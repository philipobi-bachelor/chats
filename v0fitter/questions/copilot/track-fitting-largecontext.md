Document ID: `chat-logs/2101708`

<blockquote>

#### philipobi:
#codebase Is it possible to fit a `RecoTrack` for multiple particle hypotheses at once?

Variables: `codebase`

</blockquote>

<blockquote>

#### GitHub Copilot (claude-sonnet-4):

<blockquote>

Searched codebase for "#codebase Is it possible to fit a `RecoTrack` for multiple particle hypotheses at once?", 36 results

<details>

`alignment/modules/CopyRecoTracksWithOverlap/include/CopyRecoTracksWithOverlapModule.h:43:1-53:1`  
`alignment/modules/MillepedeCollector/src/MillepedeCollectorModule.cc:1038:1-1239:1`  
`alignment/modules/MillepedeCollector/src/MillepedeCollectorModule.cc:1259:1-1279:1`  
`analysis/modules/ParticleLoader/src/ParticleLoaderModule.cc:650:1-666:1`  
`analysis/modules/TagVertex/src/TagVertexModule.cc:613:1-618:1`  
`analysis/scripts/modularAnalysis.py:1066:1-1101:1`  
`analysis/VertexFitting/TreeFitter/include/RecoTrack.h:20:1-43:1`  
`analysis/VertexFitting/TreeFitter/include/RecoTrack.h:71:1-88:1`  
`analysis/VertexFitting/TreeFitter/src/RecoTrack.cc:20:1-68:1`  
`mdst/dataobjects/include/Track.h:86:1-92:1`  
`mdst/dataobjects/include/Track.h:100:1-109:1`  
`online_book/fundamentals/04-reconstruction.rst:146:1-166:1`  
`tracking/dataobjects/include/RecoTrack.h:474:1-488:1`  
`tracking/dataobjects/include/RecoTrack.h:1105:1-1113:1`  
`tracking/dataobjects/src/RecoTrack.cc:1:1-27:1`  
`tracking/doc/trackParameterization.rst:30:1-41:1`  
`tracking/doc/trackParameterization.rst:48:1-58:1`  
`tracking/examples/VXDTF_redesign/eventSimulation.py:160:1-176:1`  
`tracking/kinkFinding/fitter/src/KinkFitter.cc:118:1-358:1`  
`tracking/modules/fitter/include/BaseRecoFitterModule.h:56:1-68:1`  
`tracking/modules/fitter/src/BaseRecoFitterModule.cc:162:1-173:1`  
`tracking/modules/recoTrackParticleLoader/include/RecoTrackParticleLoaderModule.h:22:1-57:1`  
`tracking/modules/recoTrackParticleLoader/src/RecoTrackParticleLoaderModule.cc:20:1-34:1`  
`tracking/modules/trackCreator/include/TrackCreatorModule.h:23:1-79:1`  
`tracking/modules/trackFinderMCTruth/src/TrackFinderMCTruthRecoTracksModule.cc:864:1-879:1`  
`tracking/scripts/tracking/harvest/peelers.py:642:1-665:1`  
`tracking/scripts/tracking/validation/utilities.py:172:1-196:1`  
`tracking/trackFindingCDC/eventdata/utils/include/RecoTrackUtil.h:22:1-46:1`  
`tracking/trackFitting/fitter/base/include/TrackFitter.h:196:1-227:1`  
`tracking/trackFitting/fitter/base/src/TrackFitter.cc:45:1-70:1`  
`tracking/trackFitting/trackBuilder/factories/src/TrackBuilder.cc:37:1-71:1`  
`tracking/trackFitting/trackBuilder/factories/src/TrackBuilder.cc:126:1-139:1`  
`tracking/v0Finding/fitter/src/NewV0Fitter.cc:110:1-121:1`  
`tracking/v0Finding/fitter/src/NewV0Fitter.cc:199:1-226:1`  
`tracking/v0Finding/fitter/src/NewV0Fitter.cc:311:1-339:1`  
`tracking/v0Finding/fitter/src/NewV0Fitter.cc:373:1-393:1`

</details>

</blockquote>

Based on the codebase, yes, it is possible to fit a `RecoTrack` for multiple particle hypotheses, but this is typically done sequentially rather than simultaneously. Here's how this works:

## Multiple Hypothesis Fitting

The `BaseRecoFitterModule` demonstrates this pattern. It can fit a RecoTrack with multiple particle hypotheses by:

1. **Sequential Fitting**: The module iterates through different particle hypotheses and fits each one:

````cpp
// if charge has been flipped reset seed charge and refit all track representations
if (flippedCharge) {
  recoTrack.setChargeSeed(-recoTrack.getChargeSeed());
  // refit all present track representations
  for (const auto trackRep : recoTrack.getRepresentations()) {
    Const::ChargedStable particleUsedForFitting(abs(trackRep->getPDG()));
    fitter.fit(recoTrack, particleUsedForFitting);
  }
}
````

2. **Track Representations**: Each particle hypothesis creates a separate track representation within the same RecoTrack. As noted in `RecoTrackGenfitAccess`:

> "By convention, only one TrackRep for one particle type can exist inside of a RecoTrack, no matter the charge. So there can only be a electron or positron TrackRep, but not both."

## How It Works

1. **Adding Hypotheses**: Different particle hypotheses are added as separate track representations using `TrackFitter::fit`:

````cpp
bool TrackFitter::fit(RecoTrack& recoTrack, const Const::ChargedStable& particleType, bool resortHits) const
{
  const int currentPdgCode = TrackFitter::createCorrectPDGCodeForChargedStable(particleType, recoTrack);
  genfit::AbsTrackRep* trackRepresentation = RecoTrackGenfitAccess::createOrReturnRKTrackRep(recoTrack, currentPdgCode);
  return fit(recoTrack, trackRepresentation, resortHits);
}
````

2. **Storage**: The `TrackBuilder` then converts these multiple representations into separate `TrackFitResult` objects stored in the final `Track`:

````cpp
for (const auto& trackRep : trackReps) {
  // Check if the fitted particle type is in our charged stable set
  const Const::ParticleType particleType(std::abs(trackRep->getPDG()));
  // ... fit and store each hypothesis
}
````

## Practical Usage

This multi-hypothesis approach is used throughout the framework:
- **Track reconstruction** fits common hypotheses (π, K, p, e, μ)
- **Analysis modules** can request specific hypotheses via `fillParticleListWithTrackHypothesis`
- **Track selection** can choose the best-fitting hypothesis using `getTrackFitResultWithClosestMass`

So while not truly simultaneous, the framework efficiently handles multiple particle hypotheses for the same set of hits within a single RecoTrack object.

`(25.279 s)`

</blockquote>

