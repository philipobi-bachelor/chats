Document ID: `chat-logs/2101497`

<blockquote>

#### philipobi:
#file:RecoTrack.h  #file:RecoTrack.cc  #file:TrackFitter.h  #file:TrackFitter.cc  is it possible to fit a track for multiple particle hypotheses at once?

Variables: `file:TrackFitter.cc`, `file:TrackFitter.h`, `file:RecoTrack.cc`, `file:RecoTrack.h`

</blockquote>

<blockquote>

#### GitHub Copilot (claude-sonnet-4):

Looking at the code, yes, it is possible to fit a track for multiple particle hypotheses at once, but with some important caveats:

## How it works:

1. **Multiple TrackReps can be added**: The RecoTrack can hold multiple `genfit::AbsTrackRep` objects, each representing a different particle hypothesis (e.g., pion, kaon, proton, electron).

2. **All representations are fitted together**: In `TrackFitter::fitWithoutCheck()`, there's this key comment:
   ```cpp
   // In every fit step, all track representations are fitted with genfit.
   ```

3. **Adding multiple hypotheses**: You can add multiple particle hypotheses by calling the fit function multiple times with different particle types:

```cpp
TrackFitter trackFitter;
RecoTrack& recoTrack = /* your track */;

// Fit with pion hypothesis
trackFitter.fit(recoTrack, Const::pion);

// Fit with kaon hypothesis  
trackFitter.fit(recoTrack, Const::kaon);

// Fit with proton hypothesis
trackFitter.fit(recoTrack, Const::proton);
```

## Important limitations:

1. **Same charge constraint**: From `RecoTrackGenfitAccess::createOrReturnRKTrackRep()`, only one TrackRep per particle type can exist, and it must match the track's charge. The comment states:
   ```cpp
   // By convention, only one TrackRep for one particle type can exist
   // inside of a RecoTrack, no matter the charge. So there can only be a 
   // electron or positron TrackRep, but not both.
   ```

2. **Cardinal representation**: Only one representation can be the "cardinal" (primary) representation at a time.

3. **Efficiency consideration**: Each call to `fit()` with a new particle hypothesis triggers a complete refit of all representations, so it's computationally expensive.

## Best practice:

For fitting multiple hypotheses efficiently, you would typically:

```cpp
TrackFitter trackFitter;

// Add all desired hypotheses first (this creates the TrackReps)
genfit::AbsTrackRep* pionRep = RecoTrackGenfitAccess::createOrReturnRKTrackRep(recoTrack, 211);  // pion
genfit::AbsTrackRep* kaonRep = RecoTrackGenfitAccess::createOrReturnRKTrackRep(recoTrack, 321);  // kaon
genfit::AbsTrackRep* protonRep = RecoTrackGenfitAccess::createOrReturnRKTrackRep(recoTrack, 2212); // proton

// Then fit once (all representations will be fitted together)
trackFitter.fit(recoTrack, pionRep);  // This fits all representations
```

So yes, multiple particle hypotheses can be fitted simultaneously, which is useful for particle identification and track quality assessment.

`(16.230 s)`

</blockquote>

