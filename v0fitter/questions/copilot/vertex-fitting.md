Document ID: `chat-logs/2101564`

<blockquote>

#### philipobi:
#file:GFRaveVertexFactory.cc #file:GFRaveVertexFactory.h Is it possible to perform the vertex fit for multiple track representations at once?

Variables: `file:GFRaveVertexFactory.h`, `file:GFRaveVertexFactory.cc`

</blockquote>

<blockquote>

#### GitHub Copilot (claude-sonnet-4):

Looking at the code, **no, it's not possible to perform vertex fitting for multiple track representations at once** with the current implementation.

Here's why:

## Current Limitations

1. **Single State per Track**: The `findVertices` methods only accept either:
   - Track objects (using their fitted states)
   - A single `MeasuredStateOnPlane` per track

2. **No Multi-Representation Support**: The conversion function `GFTracksToTracks` (called in both `findVertices` methods) doesn't have any mechanism to handle multiple representations per track.

3. **Map Structure**: The `IdGFTrackStateMap_` uses a simple `std::map<int, genfit::trackAndState>` where each track ID maps to a single state, not multiple states.

## What You'd Need to Modify

To support multiple representations, you would need to:

1. **Extend the interface** to accept multiple states per track:
```cpp
void findVertices(std::vector<genfit::GFRaveVertex*>*, 
                  const std::vector<genfit::Track*>&, 
                  std::vector<std::vector<genfit::MeasuredStateOnPlane*>>&, // Multiple states per track
                  bool use_beamspot=false);
```

2. **Modify the data structure**:
```cpp
std::map<int, std::vector<genfit::trackAndState>> IdGFTrackStateMap_; // Multiple states per ID
```

3. **Update the conversion logic** in `GFTracksToTracks` to handle multiple representations and decide which one to use for the vertex fit.

## Current Workflow

The current implementation follows this path:
1. Takes one state per track
2. Converts to rave format via `GFTracksToTracks`
3. Passes to `rave::VertexFactory::create`
4. Converts results back via `RaveToGFVertices`

Each track contributes exactly one measurement to the vertex fit, so you'd need to choose which representation to use beforehand.

`(12.134 s)`

</blockquote>

