Document ID: `chat-logs/1498992`

<blockquote>

#### philipobi:
Add Doxygen-style documentation to #file:NewV0Fitter.h  Follow the conventions outlined in #file:doxygen-overview.md  and the relevant section of coding_conventions.rst. Refer to #file:NewV0Fitter.cc  for implementation details, but do not modify it. Keep the documentation concise, meaningful, and strictly comment-based—do not alter any code logic.

Variables: `file:NewV0Fitter.cc`, `file:doxygen-overview.md`, `file:NewV0Fitter.h`, `coding_conventions.rst`

</blockquote>

<blockquote>

#### GitHub Copilot (claude-sonnet-4):

I'll add Doxygen-style documentation to the NewV0Fitter.h header file, following the Belle II coding conventions and the Doxygen style guide. Let me analyze the code structure and add comprehensive documentation. 

<blockquote>

Edited `tracking/v0Finding/fitter/include/NewV0Fitter.h`

<details>

```diff
--- a/tracking/v0Finding/fitter/include/NewV0Fitter.h
+++ b/tracking/v0Finding/fitter/include/NewV0Fitter.h
@@ -5,6 +5,14 @@
  * See git log for contributors and copyright holders.                    *
  * This file is licensed under LGPL-3.0, see LICENSE.md.                  *
  **************************************************************************/
+
+/** @file NewV0Fitter.h
+ * New V0 finding and fitting algorithm implementation.
+ * 
+ * This file contains classes for fitting V0 vertices from track pairs,
+ * including track management, vertex fitting, and validation functionality.
+ */
+
 #pragma once
 
 #include <genfit/AbsTrackRep.h>
```

</details>

</blockquote>

<blockquote>

Edited `tracking/v0Finding/fitter/include/NewV0Fitter.h`

<details>

```diff
--- a/tracking/v0Finding/fitter/include/NewV0Fitter.h
+++ b/tracking/v0Finding/fitter/include/NewV0Fitter.h
@@ -40,8 +40,21 @@
 #include <vector>
 
 namespace Belle2 {
+  /**
+   * Namespace containing the new V0 finding and fitting algorithm.
+   * 
+   * This namespace provides a complete framework for V0 particle reconstruction,
+   * including track fitting, vertex finding, and validation tools for particles
+   * like K_short, Lambda, and photon conversions.
+   */
   namespace NewV0Fitter {
 
+    /**
+     * Configuration parameters for the V0 fitter.
+     * 
+     * Contains all settings needed to configure the V0 finding algorithm,
+     * including fitting modes, validation options, mass ranges, and array names.
+     */
     struct Config {
     public:
       Config(int v0FitterMode, bool enableValidation, double beamPipeRadius,
```

</details>

</blockquote>

<blockquote>

Edited `tracking/v0Finding/fitter/include/NewV0Fitter.h`

<details>

```diff
--- a/tracking/v0Finding/fitter/include/NewV0Fitter.h
+++ b/tracking/v0Finding/fitter/include/NewV0Fitter.h
@@ -55,8 +55,30 @@
      * Contains all settings needed to configure the V0 finding algorithm,
      * including fitting modes, validation options, mass ranges, and array names.
      */
+    /**
+     * Configuration parameters for the V0 fitter.
+     * 
+     * Contains all settings needed to configure the V0 finding algorithm,
+     * including fitting modes, validation options, mass ranges, and array names.
+     */
     struct Config {
     public:
+      /**
+       * Constructor for V0 fitter configuration.
+       * 
+       * @param v0FitterMode Operating mode of the V0 fitter (0, 1, or 2)
+       * @param enableValidation Whether to enable validation vertex creation
+       * @param beamPipeRadius Radius of the beam pipe for geometric cuts
+       * @param vertexChi2CutOutside Chi2 cut for vertices outside beam pipe
+       * @param invMassRangeKshort Invariant mass range for K_short hypothesis
+       * @param invMassRangeLambda Invariant mass range for Lambda hypothesis  
+       * @param invMassRangePhoton Invariant mass range for photon hypothesis
+       * @param arrayNameTFResults Name of TrackFitResult array in DataStore
+       * @param arrayNameV0s Name of V0 array in DataStore
+       * @param arrayNameV0ValidationVertices Name of validation vertex array
+       * @param arrayNameRecoTracks Name of RecoTrack array in DataStore
+       * @param arrayNameCopiedRecoTracks Name of copied RecoTrack array
+       */
       Config(int v0FitterMode, bool enableValidation, double beamPipeRadius,
              double vertexChi2CutOutside,
              const std::tuple<double, double>& invMassRangeKshort,
@@ -68,22 +90,22 @@
              const std::string& arrayNameCopiedRecoTracks);
       
       
-      const std::string arrayNameTFResults;
-      const std::string arrayNameV0s;
-      const std::string arrayNameV0ValidationVertices;
-      const std::string arrayNameRecoTracks;
-      const std::string arrayNameCopiedRecoTracks;
-      
-      const std::tuple<double, double> invMassRangeKshort;
-      const std::tuple<double, double> invMassRangeLambda;
-      const std::tuple<double, double> invMassRangePhoton;
-      const double beamPipeRadius;
-      const double vertexChi2CutOutside;
-
-      const int v0FitterMode;
-      const bool forcestore;
-      const bool useOnlyOneSVDHitPair;
-      const bool enableValidation;
+      const std::string arrayNameTFResults; ///< Name of TrackFitResult array in DataStore
+      const std::string arrayNameV0s; ///< Name of V0 array in DataStore
+      const std::string arrayNameV0ValidationVertices; ///< Name of validation vertex array
+      const std::string arrayNameRecoTracks; ///< Name of RecoTrack array in DataStore
+      const std::string arrayNameCopiedRecoTracks; ///< Name of copied RecoTrack array
+      
+      const std::tuple<double, double> invMassRangeKshort; ///< Invariant mass range for K_short hypothesis
+      const std::tuple<double, double> invMassRangeLambda; ///< Invariant mass range for Lambda hypothesis
+      const std::tuple<double, double> invMassRangePhoton; ///< Invariant mass range for photon hypothesis
+      const double beamPipeRadius; ///< Radius of the beam pipe for geometric cuts
+      const double vertexChi2CutOutside; ///< Chi2 cut for vertices outside beam pipe
+
+      const int v0FitterMode; ///< Operating mode of the V0 fitter (0, 1, or 2)
+      const bool forcestore; ///< Whether to force storage of V0 candidates
+      const bool useOnlyOneSVDHitPair; ///< Whether to use only one SVD hit pair per track
+      const bool enableValidation; ///< Whether to enable validation vertex creation
     };
 
     class Storage {
```

</details>

</blockquote>

<blockquote>

Edited `tracking/v0Finding/fitter/include/NewV0Fitter.h`

<details>

```diff
--- a/tracking/v0Finding/fitter/include/NewV0Fitter.h
+++ b/tracking/v0Finding/fitter/include/NewV0Fitter.h
@@ -108,22 +108,33 @@
       const bool enableValidation; ///< Whether to enable validation vertex creation
     };
 
+    /**
+     * Storage management class for V0 fitter DataStore arrays.
+     * 
+     * Manages initialization and access to all DataStore arrays used by the V0 fitter,
+     * including track fit results, V0 objects, validation vertices, and track collections.
+     */
     class Storage {
     public:
+      /**
+       * Initialize all DataStore arrays based on configuration.
+       * 
+       * @param config Configuration object containing array names and settings
+       */
       static void init(const Config& config);
       
-      static StoreArray<TrackFitResult>* TFResults;
-      static StoreArray<Belle2::V0>* V0s;
-      static std::optional<StoreArray<V0ValidationVertex> *> V0ValidationVertices;
-      static const StoreArray<RecoTrack>* RecoTracks;
-      static StoreArray<RecoTrack>* CopiedRecoTracks;
+      static StoreArray<TrackFitResult>* TFResults; ///< Pointer to TrackFitResult array
+      static StoreArray<Belle2::V0>* V0s; ///< Pointer to V0 array
+      static std::optional<StoreArray<V0ValidationVertex> *> V0ValidationVertices; ///< Optional pointer to validation vertex array
+      static const StoreArray<RecoTrack>* RecoTracks; ///< Pointer to input RecoTrack array
+      static StoreArray<RecoTrack>* CopiedRecoTracks; ///< Pointer to copied RecoTrack array
 
     private:
-      static StoreArray<TrackFitResult> s_arrayTFResults;
-      static StoreArray<Belle2::V0> s_arrayV0s;
-      static StoreArray<V0ValidationVertex> s_arrayV0ValidationVertices;
-      static StoreArray<RecoTrack> s_arrayRecoTracks;
-      static StoreArray<RecoTrack> s_arrayCopiedRecoTracks;
+      static StoreArray<TrackFitResult> s_arrayTFResults; ///< Static TrackFitResult array instance
+      static StoreArray<Belle2::V0> s_arrayV0s; ///< Static V0 array instance
+      static StoreArray<V0ValidationVertex> s_arrayV0ValidationVertices; ///< Static validation vertex array instance
+      static StoreArray<RecoTrack> s_arrayRecoTracks; ///< Static input RecoTrack array instance
+      static StoreArray<RecoTrack> s_arrayCopiedRecoTracks; ///< Static copied RecoTrack array instance
     };
 
     class Track;
```

</details>

</blockquote>

<blockquote>

Edited `tracking/v0Finding/fitter/include/NewV0Fitter.h`

<details>

```diff
--- a/tracking/v0Finding/fitter/include/NewV0Fitter.h
+++ b/tracking/v0Finding/fitter/include/NewV0Fitter.h
@@ -140,23 +140,59 @@
     class Track;
     class V0;
 
+    /**
+     * Main V0 vertex fitter class.
+     * 
+     * Handles the complete V0 fitting workflow including vertex fitting,
+     * track parameter extraction, and storage of results. Supports multiple
+     * V0 hypotheses (K_short, Lambda, photon conversions).
+     */
     class Fitter {
     public:
+      /**
+       * Constructor for V0 fitter.
+       * 
+       * @param config Configuration object with fitter settings
+       */
       Fitter(const Config& config);
 
+      /**
+       * Fit a V0 vertex from two tracks and store the result.
+       * 
+       * @param trackPlus Positive track for V0 fitting
+       * @param trackMinus Negative track for V0 fitting  
+       * @param v0Hypothesis Particle hypothesis for the V0 (K_short, Lambda, etc.)
+       * @param isForceStored Output flag indicating if V0 was force-stored
+       * @param isHitRemoved Output flag indicating if hits were removed during fitting
+       * @return True if fitting and storage was successful
+       */
       bool fitAndStore(const Belle2::Track* trackPlus,
                        const Belle2::Track* trackMinus,
                        const Const::ParticleType& v0Hypothesis, bool& isForceStored,
                        bool& isHitRemoved);
 
+      /**
+       * Get track particle hypotheses for a given V0 hypothesis.
+       * 
+       * @param v0Hypothesis V0 particle type (K_short, Lambda, photon)
+       * @return Pair of particle hypotheses for positive and negative tracks
+       */
       static std::pair<Const::ParticleType, Const::ParticleType>
       getTrackHypotheses(const Const::ParticleType& v0Hypothesis);
     
     private:
+      /**
+       * Fit vertex and apply pre-selection cuts.
+       * 
+       * @param v0 V0 object to fit
+       * @param trackPlus Positive track
+       * @param trackMinus Negative track
+       * @return True if vertex fitting and cuts passed
+       */
       bool fitVertexAndPrecut(V0& v0, Track& trackPlus, Track& trackMinus);
       
-      const Config m_config;
-      genfit::GFRaveVertexFactory m_GFRaveVertexFactory;
+      const Config m_config; ///< Configuration parameters
+      genfit::GFRaveVertexFactory m_GFRaveVertexFactory; ///< Vertex factory for fitting
     };
 
     class V0 {
```

</details>

</blockquote>

<blockquote>

Edited `tracking/v0Finding/fitter/include/NewV0Fitter.h`

<details>

```diff
--- a/tracking/v0Finding/fitter/include/NewV0Fitter.h
+++ b/tracking/v0Finding/fitter/include/NewV0Fitter.h
@@ -195,31 +195,116 @@
       genfit::GFRaveVertexFactory m_GFRaveVertexFactory; ///< Vertex factory for fitting
     };
 
+    /**
+     * V0 particle candidate class.
+     * 
+     * Represents a V0 particle candidate with its decay products, vertex information,
+     * and associated validation data. Handles creation, fitting, and storage of V0 objects
+     * for different particle hypotheses (K_short, Lambda, photon conversions).
+     */
     class V0 {
       friend Fitter::Fitter(const Config& config);
     public:
+      /// Pair containing V0 object and optional validation vertex
       typedef std::pair <
         std::unique_ptr<Belle2::V0>,
         std::unique_ptr<Belle2::V0ValidationVertex>
       > V0ObjPair;
+      /// Pair of particle types for the two daughter tracks
       typedef std::pair<Const::ParticleType, Const::ParticleType> DaughterPair;
 
-      bool invMassCut(const uint tolerance = 0) const;      
+      /**
+       * Check if V0 candidate passes invariant mass cuts.
+       * 
+       * @param tolerance Additional tolerance for mass window (default: 0)
+       * @return True if candidate passes mass cuts
+       */
+      bool invMassCut(const uint tolerance = 0) const;
+      
+      /**
+       * Temporarily store V0 object pair for later processing.
+       * 
+       * @param v0ObjPair Pair of V0 object and validation vertex to stash
+       */
       void stash(V0ObjPair&& v0ObjPair);
+      
+      /**
+       * Retrieve and store previously stashed V0 object.
+       * 
+       * @return True if stashed object was successfully stored
+       */
       bool stashPop();
+      
+      /**
+       * Get daughter particle hypotheses for this V0.
+       * 
+       * @return Pair of particle types for positive and negative tracks
+       */
       const DaughterPair &getDaughterParticles() const;
+      
+      /**
+       * Get vertex position of the V0.
+       * 
+       * @return 3D position vector of the vertex
+       */
       const ROOT::Math::XYZVector& getVertexPos() const;
+      
+      /**
+       * Get the fitted vertex object.
+       * 
+       * @return Reference to the GFRave vertex
+       */
       const genfit::GFRaveVertex &getGFRaveVertex() const;
+      
+      /**
+       * Set the fitted vertex for this V0.
+       * 
+       * @param gfRaveVertex Fitted vertex object
+       */
       void setVertex(genfit::GFRaveVertex &gfRaveVertex);
+      
+      /**
+       * Set the 4-momentum of the V0 at the vertex.
+       * 
+       * @param lorentzVectorAtVertex 4-momentum vector at vertex
+       */
       void setLorentzVectorAtVertex(ROOT::Math::PxPyPzMVector &&lorentzVectorAtVertex);
       
+      /**
+       * Store V0 object pair in DataStore.
+       * 
+       * @param v0ObjPair Pair containing V0 and validation vertex objects
+       */
       static void store(V0ObjPair&& v0ObjPair);
+      
+      /**
+       * Build V0 and validation vertex objects from fitted data.
+       * 
+       * @param v0 V0 candidate with fitted vertex
+       * @param trackPlus Positive daughter track
+       * @param trackMinus Negative daughter track
+       * @return Pair of created V0 objects
+       */
       static V0ObjPair buildObj(
         const V0& v0, 
         const Track& trackPlus,
         const Track& trackMinus
       );
+      
+      /**
+       * Create V0 candidate for given particle hypothesis.
+       * 
+       * @param v0Hypothesis Particle type hypothesis (K_short, Lambda, photon)
+       * @return V0 candidate object
+       */
       static V0 create(const Const::ParticleType& v0Hypothesis);
+      
+      /**
+       * Get daughter particle hypotheses for given V0 hypothesis.
+       * 
+       * @param v0Hypothesis V0 particle type
+       * @return Pair of daughter particle hypotheses
+       */
       static const DaughterPair getDaughterParticles(const Const::ParticleType& v0Hypothesis);
       
     private:  
```

</details>

</blockquote>

<blockquote>

Edited `tracking/v0Finding/fitter/include/NewV0Fitter.h`

<details>

```diff
--- a/tracking/v0Finding/fitter/include/NewV0Fitter.h
+++ b/tracking/v0Finding/fitter/include/NewV0Fitter.h
@@ -308,33 +308,52 @@
       static const DaughterPair getDaughterParticles(const Const::ParticleType& v0Hypothesis);
       
     private:  
+      /**
+       * Temporary storage for V0 objects during processing.
+       * 
+       * Holds V0 and validation vertex objects that can be retrieved
+       * and stored later in the processing workflow.
+       */
       struct V0ObjStash {
-        std::unique_ptr<Belle2::V0> m_V0;
-        std::unique_ptr<V0ValidationVertex> m_ValidationV0;
+        std::unique_ptr<Belle2::V0> m_V0; ///< Stored V0 object
+        std::unique_ptr<V0ValidationVertex> m_ValidationV0; ///< Stored validation vertex
+        
+        /**
+         * Constructor from V0 object pair.
+         * 
+         * @param v0ObjPair Pair containing V0 and validation objects
+         */
         V0ObjStash(V0ObjPair&& v0ObjPair) : 
           m_V0(std::move(std::get<0>(v0ObjPair))),
           m_ValidationV0(std::move(std::get<1>(v0ObjPair)))
         {}
       };
       
+      /**
+       * Constructor for V0 candidate.
+       * 
+       * @param V0Particle Particle type of the V0
+       * @param V0InvMassRange Allowed invariant mass range
+       * @param daughterParticles Particle types of daughter tracks
+       */
       V0(const Const::ParticleType& V0Particle,
          const std::tuple<double, double>& V0InvMassRange,
          const DaughterPair& daughterParticles);
       
-      std::optional<V0ObjStash> m_v0ObjStash;
-      
-      const Const::ParticleType& m_V0Particle;
-      const std::tuple<double, double>& m_V0InvMassRange;
-      const DaughterPair& m_daughterParticles;
-      genfit::GFRaveVertex m_GFRaveVertex;
-      ROOT::Math::XYZVector m_vertexPos;
-      ROOT::Math::PxPyPzMVector m_lorentzVectorAtVertex;
-
-      static const Config* s_config;
-      static const DaughterPair KshortDaughters;
-      static const DaughterPair PhotonDaughters;
-      static const DaughterPair LambdaDaughters;
-      static const DaughterPair AntiLambdaDaughters;
+      std::optional<V0ObjStash> m_v0ObjStash; ///< Optional temporary storage for V0 objects
+      
+      const Const::ParticleType& m_V0Particle; ///< Particle type of this V0
+      const std::tuple<double, double>& m_V0InvMassRange; ///< Invariant mass range for this V0 type
+      const DaughterPair& m_daughterParticles; ///< Particle types of daughter tracks
+      genfit::GFRaveVertex m_GFRaveVertex; ///< Fitted vertex object
+      ROOT::Math::XYZVector m_vertexPos; ///< Position of the fitted vertex
+      ROOT::Math::PxPyPzMVector m_lorentzVectorAtVertex; ///< 4-momentum at vertex
+
+      static const Config* s_config; ///< Pointer to global configuration
+      static const DaughterPair KshortDaughters; ///< K_short daughter particle types (pi+, pi-)
+      static const DaughterPair PhotonDaughters; ///< Photon daughter particle types (e+, e-)
+      static const DaughterPair LambdaDaughters; ///< Lambda daughter particle types (p+, pi-)
+      static const DaughterPair AntiLambdaDaughters; ///< Anti-Lambda daughter particle types (pi+, p-)
     };
 
     class TrackContainer {
```

</details>

</blockquote>

<blockquote>

Edited `tracking/v0Finding/fitter/include/NewV0Fitter.h`

<details>

```diff
--- a/tracking/v0Finding/fitter/include/NewV0Fitter.h
+++ b/tracking/v0Finding/fitter/include/NewV0Fitter.h
@@ -356,34 +356,87 @@
       static const DaughterPair AntiLambdaDaughters; ///< Anti-Lambda daughter particle types (pi+, p-)
     };
 
+    /**
+     * Container class for RecoTrack with genfit Track access.
+     * 
+     * Provides a wrapper around RecoTrack objects with convenient access to
+     * the underlying genfit Track and track representation. Handles track
+     * copying, fitting preparation, and hit information access.
+     */
     class TrackContainer {
       friend Track;
     public:
+      /**
+       * Constructor for track container.
+       * 
+       * @param recoTrack Pointer to the RecoTrack object to wrap
+       * @param pdgClosest PDG code of the closest mass hypothesis
+       * @param isOriginal Whether this is the original track (true) or a copy (false)
+       */
       TrackContainer(
           RecoTrack* recoTrack,
           const uint pdgClosest,
           const bool isOriginal = true
       );
 
+      /**
+       * Destructor handles track representation restoration.
+       */
       ~TrackContainer();
 
     private:
+      /**
+       * Prepare track for vertex fitting by selecting appropriate track representation.
+       * 
+       * @return True if preparation was successful
+       */
       bool prepareVertexFit();
+      
+      /**
+       * Fit the track with current settings.
+       * 
+       * @return True if fitting was successful
+       */
       bool fit();
+      
+      /**
+       * Get hit information objects for this track.
+       * 
+       * @return Vector of pointers to RecoHitInformation objects
+       */
       const std::vector<RecoHitInformation*> getHitInfos() const;
+      
+      /**
+       * Get measured state on plane from first hit.
+       * 
+       * @return Measured state on plane at first hit
+       */
       genfit::MeasuredStateOnPlane getMSOPfromFirstHit() const;
+      
+      /**
+       * Get measured state on plane from specific hit.
+       * 
+       * @param hitInfo Pointer to specific hit information
+       * @return Measured state on plane at specified hit
+       */
       genfit::MeasuredStateOnPlane getMSOPfromHit(const RecoHitInformation* hitInfo) const;
       
+      /**
+       * Create a copy of the track container.
+       * 
+       * @param trackContainer Original track container to copy
+       * @return Unique pointer to copied track container
+       */
       static std::unique_ptr<TrackContainer> copy(const TrackContainer &trackContainer);
       
-      const genfit::AbsTrackRep* m_trackRep = nullptr;
-      RecoTrack* m_recoTrack = nullptr;
-      genfit::Track &m_genfitTrack;
-      const uint m_OriginalCardinalRepID = 0;
-      const uint m_pdgClosest = 0;
-      const Const::ChargedStable m_particleClosest;
-      const bool m_isOriginal = true;
-      bool m_isSmudged = false;
+      const genfit::AbsTrackRep* m_trackRep = nullptr; ///< Pointer to selected track representation
+      RecoTrack* m_recoTrack = nullptr; ///< Pointer to wrapped RecoTrack
+      genfit::Track &m_genfitTrack; ///< Reference to underlying genfit Track
+      const uint m_OriginalCardinalRepID = 0; ///< Original cardinal representation ID
+      const uint m_pdgClosest = 0; ///< PDG code of closest mass hypothesis
+      const Const::ChargedStable m_particleClosest; ///< Particle type of closest mass hypothesis
+      const bool m_isOriginal = true; ///< Whether this is the original track or a copy
+      bool m_isSmudged = false; ///< Whether track representation has been modified
     };
 
     class Track {
```

</details>

</blockquote>

<blockquote>

Edited `tracking/v0Finding/fitter/include/NewV0Fitter.h`

<details>

```diff
--- a/tracking/v0Finding/fitter/include/NewV0Fitter.h
+++ b/tracking/v0Finding/fitter/include/NewV0Fitter.h
@@ -439,32 +439,129 @@
       bool m_isSmudged = false; ///< Whether track representation has been modified
     };
 
+    /**
+     * Track wrapper class for V0 fitting.
+     * 
+     * Wraps Belle II Track objects and provides functionality for V0 fitting including
+     * hit consistency checks, inner hit removal, track refitting, and shared cluster detection.
+     * Handles both original tracks and copied tracks for hit removal algorithms.
+     */
     class Track {
       friend Fitter::Fitter(const Config&);
     public:  
+      /**
+       * Constructor for track wrapper.
+       * 
+       * @param isTrackPlus Whether this is the positive track in the V0 pair
+       * @param track Pointer to the Belle II Track object
+       * @param particleHyp Particle hypothesis for this track
+       */
       Track(
         const bool isTrackPlus,
         const Belle2::Track* track, 
         const Const::ChargedStable& particleHyp
       );
       
+      /**
+       * Prepare track for vertex fitting.
+       * 
+       * @return True if preparation was successful
+       */
       bool prepareVertexFit();
+      
+      /**
+       * Check for inner hits relative to vertex position.
+       * 
+       * @param vertexPos Position of the V0 vertex
+       * @return True if check completed successfully
+       */
       bool checkInnerHits(const ROOT::Math::XYZVector& vertexPos);
+      
+      /**
+       * Check consistency of hits on the track.
+       */
       void checkHitConsistency();
+      
+      /**
+       * Remove inner hits and refit the track.
+       * 
+       * @param vertexPos Position of the V0 vertex for determining inner hits
+       * @return True if hit removal and refitting was successful
+       */
       bool removeInnerHitsAndRefit(const ROOT::Math::XYZVector& vertexPos);
+      
+      /**
+       * Copy track and refit with current settings.
+       * 
+       * @return True if copying and refitting was successful
+       */
       bool copyAndRefit();
+      
+      /**
+       * Check if track has been refitted.
+       * 
+       * @return True if track was refitted
+       */
       bool isRefit() const;
+      
+      /**
+       * Check if track has inner hits.
+       * 
+       * @return True if track has inner hits
+       */
       bool hasInnerHit() const;
+      
+      /**
+       * Check if track has hit inconsistencies.
+       * 
+       * @return True if track has hit inconsistencies
+       */
       bool hasHitInconsistency() const;
+      
+      /**
+       * Get the underlying Belle II Track object.
+       * 
+       * @return Pointer to the Track object
+       */
       const Belle2::Track *getTrack() const;
+      
+      /**
+       * Get the underlying genfit Track object.
+       * 
+       * @return Pointer to the genfit Track
+       */
       genfit::Track *getGFTrack();
+      
+      /**
+       * Compute 4-momentum vector at vertex from track parameters.
+       * 
+       * @param trackParamsAtVertex Track parameters at the vertex
+       * @return 4-momentum vector with track mass hypothesis
+       */
       ROOT::Math::PxPyPzMVector computeLorentzVectorAtVertex(
         const genfit::GFRaveTrackParameters* trackParamsAtVertex
       ) const;
+      
+      /**
+       * Store track fit result in DataStore.
+       * 
+       * @param Bz Magnetic field strength at vertex
+       * @param sharedInnermostClusterFlag Flag indicating shared innermost clusters
+       * @return Pointer to stored TrackFitResult
+       */
       const TrackFitResult* storeTrackFitResult(
         const double Bz,
         const unsigned short sharedInnermostClusterFlag
       ) const;
+      
+      /**
+       * Check for shared innermost clusters between track pair.
+       * 
+       * @param trackPlus Positive track in the pair
+       * @param trackMinus Negative track in the pair
+       * @param sharedInnermostClusterFlag Output flag indicating shared clusters
+       * @return True if check completed successfully
+       */
       static bool checkSharedInnermostCluster(
         const Track& trackPlus, 
         const Track& trackMinus,
@@ -472,15 +569,15 @@
       );
 
   private:
-    const Const::ChargedStable m_particleHyp;
-    std::unique_ptr<TrackContainer> m_trackContainer;
-    const Belle2::Track* m_track;
-    uint m_nRemoved = 0;
-    const bool m_isTrackPlus = true;
-    bool m_hasHitInconsistency = false;
-    bool m_hasInnerHit = false;
+    const Const::ChargedStable m_particleHyp; ///< Particle hypothesis for this track
+    std::unique_ptr<TrackContainer> m_trackContainer; ///< Container for RecoTrack access
+    const Belle2::Track* m_track; ///< Pointer to Belle II Track object
+    uint m_nRemoved = 0; ///< Number of hits removed during processing
+    const bool m_isTrackPlus = true; ///< Whether this is the positive track in V0 pair
+    bool m_hasHitInconsistency = false; ///< Flag for hit inconsistencies
+    bool m_hasInnerHit = false; ///< Flag for presence of inner hits
     
-    static const Config* s_config;
+    static const Config* s_config; ///< Pointer to global configuration
     };
   }
 }
```

</details>

</blockquote>

Perfect! I've successfully added comprehensive Doxygen-style documentation to the `NewV0Fitter.h` header file. Here's a summary of what I added:

## Documentation Added:

1. **File-level documentation** with `@file` directive explaining the purpose of the file
2. **Namespace documentation** for `Belle2::NewV0Fitter` explaining the framework
3. **Class documentation** for all classes:
   - `Config`: Configuration parameters with detailed constructor documentation
   - `Storage`: DataStore array management 
   - `Fitter`: Main V0 fitting class
   - `V0`: V0 particle candidate class with comprehensive method documentation
   - `TrackContainer`: RecoTrack wrapper with genfit access
   - `Track`: Track wrapper for V0 fitting with detailed functionality description

4. **Method documentation** including:
   - `@param` tags for all input parameters
   - `@return` tags for return values
   - Brief descriptions of functionality
   - Implementation details where relevant

5. **Data member documentation** with inline comments using `/` notation

6. **Type alias documentation** for `V0ObjPair` and `DaughterPair`

7. **Nested struct documentation** for `V0ObjStash`

The documentation follows the Belle II coding conventions and Doxygen style guide:
- Uses `/**` comment blocks for classes and methods
- Uses `/` for inline member documentation  
- Includes `@param` and `@return` tags as required
- Provides meaningful descriptions of purpose and functionality
- Documents input/output parameters as specified in the conventions
- Maintains concise but informative descriptions

All documentation is comment-based and doesn't modify any code logic, as requested. The documentation will help developers understand the V0 fitting framework's architecture and how to use its various components effectively.

`(3 min, 16.455 s)`

</blockquote>

<blockquote>

##### Edited Files:

`tracking/v0Finding/fitter/include/NewV0Fitter.h`:

<details>

<summary>Squashed changes (short)</summary>

```diff
--- a/tracking/v0Finding/fitter/include/NewV0Fitter.h
+++ b/tracking/v0Finding/fitter/include/NewV0Fitter.h
@@ -5,6 +5,14 @@
  * See git log for contributors and copyright holders.                    *
  * This file is licensed under LGPL-3.0, see LICENSE.md.                  *
  **************************************************************************/
+
+/** @file NewV0Fitter.h
+ * New V0 finding and fitting algorithm implementation.
+ * 
+ * This file contains classes for fitting V0 vertices from track pairs,
+ * including track management, vertex fitting, and validation functionality.
+ */
+
 #pragma once
 
 #include <genfit/AbsTrackRep.h>
@@ -32,10 +40,45 @@
 #include <vector>
 
 namespace Belle2 {
+  /**
+   * Namespace containing the new V0 finding and fitting algorithm.
+   * 
+   * This namespace provides a complete framework for V0 particle reconstruction,
+   * including track fitting, vertex finding, and validation tools for particles
+   * like K_short, Lambda, and photon conversions.
+   */
   namespace NewV0Fitter {
 
+    /**
+     * Configuration parameters for the V0 fitter.
+     * 
+     * Contains all settings needed to configure the V0 finding algorithm,
+     * including fitting modes, validation options, mass ranges, and array names.
+     */
+    /**
+     * Configuration parameters for the V0 fitter.
+     * 
+     * Contains all settings needed to configure the V0 finding algorithm,
+     * including fitting modes, validation options, mass ranges, and array names.
+     */
     struct Config {
     public:
+      /**
+       * Constructor for V0 fitter configuration.
+       * 
+       * @param v0FitterMode Operating mode of the V0 fitter (0, 1, or 2)
+       * @param enableValidation Whether to enable validation vertex creation
+       * @param beamPipeRadius Radius of the beam pipe for geometric cuts
+       * @param vertexChi2CutOutside Chi2 cut for vertices outside beam pipe
+       * @param invMassRangeKshort Invariant mass range for K_short hypothesis
+       * @param invMassRangeLambda Invariant mass range for Lambda hypothesis  
+       * @param invMassRangePhoton Invariant mass range for photon hypothesis
+       * @param arrayNameTFResults Name of TrackFitResult array in DataStore
+       * @param arrayNameV0s Name of V0 array in DataStore
+       * @param arrayNameV0ValidationVertices Name of validation vertex array
+       * @param arrayNameRecoTracks Name of RecoTrack array in DataStore
+       * @param arrayNameCopiedRecoTracks Name of copied RecoTrack array
+       */
       Config(int v0FitterMode, bool enableValidation, double beamPipeRadius,
              double vertexChi2CutOutside,
              const std::tuple<double, double>& invMassRangeKshort,
@@ -47,177 +90,478 @@
              const std::string& arrayNameCopiedRecoTracks);
       
       
-      const std::string arrayNameTFResults;
-      const std::string arrayNameV0s;
-      const std::string arrayNameV0ValidationVertices;
-      const std::string arrayNameRecoTracks;
-      const std::string arrayNameCopiedRecoTracks;
-      
-      const std::tuple<double, double> invMassRangeKshort;
-      const std::tuple<double, double> invMassRangeLambda;
-      const std::tuple<double, double> invMassRangePhoton;
-      const double beamPipeRadius;
-      const double vertexChi2CutOutside;
-
-      const int v0FitterMode;
-      const bool forcestore;
-      const bool useOnlyOneSVDHitPair;
-      const bool enableValidation;
+      const std::string arrayNameTFResults; ///< Name of TrackFitResult array in DataStore
+      const std::string arrayNameV0s; ///< Name of V0 array in DataStore
+      const std::string arrayNameV0ValidationVertices; ///< Name of validation vertex array
+      const std::string arrayNameRecoTracks; ///< Name of RecoTrack array in DataStore
+      const std::string arrayNameCopiedRecoTracks; ///< Name of copied RecoTrack array
+      
+      const std::tuple<double, double> invMassRangeKshort; ///< Invariant mass range for K_short hypothesis
+      const std::tuple<double, double> invMassRangeLambda; ///< Invariant mass range for Lambda hypothesis
+      const std::tuple<double, double> invMassRangePhoton; ///< Invariant mass range for photon hypothesis
+      const double beamPipeRadius; ///< Radius of the beam pipe for geometric cuts
+      const double vertexChi2CutOutside; ///< Chi2 cut for vertices outside beam pipe
+
+      const int v0FitterMode; ///< Operating mode of the V0 fitter (0, 1, or 2)
+      const bool forcestore; ///< Whether to force storage of V0 candidates
+      const bool useOnlyOneSVDHitPair; ///< Whether to use only one SVD hit pair per track
+      const bool enableValidation; ///< Whether to enable validation vertex creation
     };
 
+    /**
+     * Storage management class for V0 fitter DataStore arrays.
+     * 
+     * Manages initialization and access to all DataStore arrays used by the V0 fitter,
+     * including track fit results, V0 objects, validation vertices, and track collections.
+     */
     class Storage {
     public:
+      /**
+       * Initialize all DataStore arrays based on configuration.
+       * 
+       * @param config Configuration object containing array names and settings
+       */
       static void init(const Config& config);
       
-      static StoreArray<TrackFitResult>* TFResults;
-      static StoreArray<Belle2::V0>* V0s;
-      static std::optional<StoreArray<V0ValidationVertex> *> V0ValidationVertices;
-      static const StoreArray<RecoTrack>* RecoTracks;
-      static StoreArray<RecoTrack>* CopiedRecoTracks;
+      static StoreArray<TrackFitResult>* TFResults; ///< Pointer to TrackFitResult array
+      static StoreArray<Belle2::V0>* V0s; ///< Pointer to V0 array
+      static std::optional<StoreArray<V0ValidationVertex> *> V0ValidationVertices; ///< Optional pointer to validation vertex array
+      static const StoreArray<RecoTrack>* RecoTracks; ///< Pointer to input RecoTrack array
+      static StoreArray<RecoTrack>* CopiedRecoTracks; ///< Pointer to copied RecoTrack array
 
     private:
-      static StoreArray<TrackFitResult> s_arrayTFResults;
-      static StoreArray<Belle2::V0> s_arrayV0s;
-      static StoreArray<V0ValidationVertex> s_arrayV0ValidationVertices;
-      static StoreArray<RecoTrack> s_arrayRecoTracks;
-      static StoreArray<RecoTrack> s_arrayCopiedRecoTracks;
+      static StoreArray<TrackFitResult> s_arrayTFResults; ///< Static TrackFitResult array instance
+      static StoreArray<Belle2::V0> s_arrayV0s; ///< Static V0 array instance
+      static StoreArray<V0ValidationVertex> s_arrayV0ValidationVertices; ///< Static validation vertex array instance
+      static StoreArray<RecoTrack> s_arrayRecoTracks; ///< Static input RecoTrack array instance
+      static StoreArray<RecoTrack> s_arrayCopiedRecoTracks; ///< Static copied RecoTrack array instance
     };
 
     class Track;
     class V0;
 
+    /**
+     * Main V0 vertex fitter class.
+     * 
+     * Handles the complete V0 fitting workflow including vertex fitting,
+     * track parameter extraction, and storage of results. Supports multiple
+     * V0 hypotheses (K_short, Lambda, photon conversions).
+     */
     class Fitter {
     public:
+      /**
+       * Constructor for V0 fitter.
+       * 
+       * @param config Configuration object with fitter settings
+       */
       Fitter(const Config& config);
 
+      /**
+       * Fit a V0 vertex from two tracks and store the result.
+       * 
+       * @param trackPlus Positive track for V0 fitting
+       * @param trackMinus Negative track for V0 fitting  
+       * @param v0Hypothesis Particle hypothesis for the V0 (K_short, Lambda, etc.)
+       * @param isForceStored Output flag indicating if V0 was force-stored
+       * @param isHitRemoved Output flag indicating if hits were removed during fitting
+       * @return True if fitting and storage was successful
+       */
       bool fitAndStore(const Belle2::Track* trackPlus,
                        const Belle2::Track* trackMinus,
                        const Const::ParticleType& v0Hypothesis, bool& isForceStored,
                        bool& isHitRemoved);
 
+      /**
+       * Get track particle hypotheses for a given V0 hypothesis.
+       * 
+       * @param v0Hypothesis V0 particle type (K_short, Lambda, photon)
+       * @return Pair of particle hypotheses for positive and negative tracks
+       */
       static std::pair<Const::ParticleType, Const::ParticleType>
       getTrackHypotheses(const Const::ParticleType& v0Hypothesis);
     
     private:
+      /**
+       * Fit vertex and apply pre-selection cuts.
+       * 
+       * @param v0 V0 object to fit
+       * @param trackPlus Positive track
+       * @param trackMinus Negative track
+       * @return True if vertex fitting and cuts passed
+       */
       bool fitVertexAndPrecut(V0& v0, Track& trackPlus, Track& trackMinus);
       
-      const Config m_config;
-      genfit::GFRaveVertexFactory m_GFRaveVertexFactory;
+      const Config m_config; ///< Configuration parameters
+      genfit::GFRaveVertexFactory m_GFRaveVertexFactory; ///< Vertex factory for fitting
     };
 
+    /**
+     * V0 particle candidate class.
+     * 
+     * Represents a V0 particle candidate with its decay products, vertex information,
+     * and associated validation data. Handles creation, fitting, and storage of V0 objects
+     * for different particle hypotheses (K_short, Lambda, photon conversions).
+     */
     class V0 {
       friend Fitter::Fitter(const Config& config);
     public:
+      /// Pair containing V0 object and optional validation vertex
       typedef std::pair <
         std::unique_ptr<Belle2::V0>,
         std::unique_ptr<Belle2::V0ValidationVertex>
       > V0ObjPair;
+      /// Pair of particle types for the two daughter tracks
       typedef std::pair<Const::ParticleType, Const::ParticleType> DaughterPair;
 
-      bool invMassCut(const uint tolerance = 0) const;      
+      /**
+       * Check if V0 candidate passes invariant mass cuts.
+       * 
+       * @param tolerance Additional tolerance for mass window (default: 0)
+       * @return True if candidate passes mass cuts
+       */
+      bool invMassCut(const uint tolerance = 0) const;
+      
+      /**
+       * Temporarily store V0 object pair for later processing.
+       * 
+       * @param v0ObjPair Pair of V0 object and validation vertex to stash
+       */
       void stash(V0ObjPair&& v0ObjPair);
+      
+      /**
+       * Retrieve and store previously stashed V0 object.
+       * 
+       * @return True if stashed object was successfully stored
+       */
       bool stashPop();
+      
+      /**
+       * Get daughter particle hypotheses for this V0.
+       * 
+       * @return Pair of particle types for positive and negative tracks
+       */
       const DaughterPair &getDaughterParticles() const;
+      
+      /**
+       * Get vertex position of the V0.
+       * 
+       * @return 3D position vector of the vertex
+       */
       const ROOT::Math::XYZVector& getVertexPos() const;
+      
+      /**
+       * Get the fitted vertex object.
+       * 
+       * @return Reference to the GFRave vertex
+       */
       const genfit::GFRaveVertex &getGFRaveVertex() const;
+      
+      /**
+       * Set the fitted vertex for this V0.
+       * 
+       * @param gfRaveVertex Fitted vertex object
+       */
       void setVertex(genfit::GFRaveVertex &gfRaveVertex);
+      
+      /**
+       * Set the 4-momentum of the V0 at the vertex.
+       * 
+       * @param lorentzVectorAtVertex 4-momentum vector at vertex
+       */
       void setLorentzVectorAtVertex(ROOT::Math::PxPyPzMVector &&lorentzVectorAtVertex);
       
+      /**
+       * Store V0 object pair in DataStore.
+       * 
+       * @param v0ObjPair Pair containing V0 and validation vertex objects
+       */
       static void store(V0ObjPair&& v0ObjPair);
+      
+      /**
+       * Build V0 and validation vertex objects from fitted data.
+       * 
+       * @param v0 V0 candidate with fitted vertex
+       * @param trackPlus Positive daughter track
+       * @param trackMinus Negative daughter track
+       * @return Pair of created V0 objects
+       */
       static V0ObjPair buildObj(
         const V0& v0, 
         const Track& trackPlus,
         const Track& trackMinus
       );
+      
+      /**
+       * Create V0 candidate for given particle hypothesis.
+       * 
+       * @param v0Hypothesis Particle type hypothesis (K_short, Lambda, photon)
+       * @return V0 candidate object
+       */
       static V0 create(const Const::ParticleType& v0Hypothesis);
+      
+      /**
+       * Get daughter particle hypotheses for given V0 hypothesis.
+       * 
+       * @param v0Hypothesis V0 particle type
+       * @return Pair of daughter particle hypotheses
+       */
       static const DaughterPair getDaughterParticles(const Const::ParticleType& v0Hypothesis);
       
     private:  
+      /**
+       * Temporary storage for V0 objects during processing.
+       * 
+       * Holds V0 and validation vertex objects that can be retrieved
+       * and stored later in the processing workflow.
+       */
       struct V0ObjStash {
-        std::unique_ptr<Belle2::V0> m_V0;
-        std::unique_ptr<V0ValidationVertex> m_ValidationV0;
+        std::unique_ptr<Belle2::V0> m_V0; ///< Stored V0 object
+        std::unique_ptr<V0ValidationVertex> m_ValidationV0; ///< Stored validation vertex
+        
+        /**
+         * Constructor from V0 object pair.
+         * 
+         * @param v0ObjPair Pair containing V0 and validation objects
+         */
         V0ObjStash(V0ObjPair&& v0ObjPair) : 
           m_V0(std::move(std::get<0>(v0ObjPair))),
           m_ValidationV0(std::move(std::get<1>(v0ObjPair)))
         {}
       };
       
+      /**
+       * Constructor for V0 candidate.
+       * 
+       * @param V0Particle Particle type of the V0
+       * @param V0InvMassRange Allowed invariant mass range
+       * @param daughterParticles Particle types of daughter tracks
+       */
       V0(const Const::ParticleType& V0Particle,
          const std::tuple<double, double>& V0InvMassRange,
          const DaughterPair& daughterParticles);
       
-      std::optional<V0ObjStash> m_v0ObjStash;
-      
-      const Const::ParticleType& m_V0Particle;
-      const std::tuple<double, double>& m_V0InvMassRange;
-      const DaughterPair& m_daughterParticles;
-      genfit::GFRaveVertex m_GFRaveVertex;
-      ROOT::Math::XYZVector m_vertexPos;
-      ROOT::Math::PxPyPzMVector m_lorentzVectorAtVertex;
-
-      static const Config* s_config;
-      static const DaughterPair KshortDaughters;
-      static const DaughterPair PhotonDaughters;
-      static const DaughterPair LambdaDaughters;
-      static const DaughterPair AntiLambdaDaughters;
+      std::optional<V0ObjStash> m_v0ObjStash; ///< Optional temporary storage for V0 objects
+      
+      const Const::ParticleType& m_V0Particle; ///< Particle type of this V0
+      const std::tuple<double, double>& m_V0InvMassRange; ///< Invariant mass range for this V0 type
+      const DaughterPair& m_daughterParticles; ///< Particle types of daughter tracks
+      genfit::GFRaveVertex m_GFRaveVertex; ///< Fitted vertex object
+      ROOT::Math::XYZVector m_vertexPos; ///< Position of the fitted vertex
+      ROOT::Math::PxPyPzMVector m_lorentzVectorAtVertex; ///< 4-momentum at vertex
+
+      static const Config* s_config; ///< Pointer to global configuration
+      static const DaughterPair KshortDaughters; ///< K_short daughter particle types (pi+, pi-)
+      static const DaughterPair PhotonDaughters; ///< Photon daughter particle types (e+, e-)
+      static const DaughterPair LambdaDaughters; ///< Lambda daughter particle types (p+, pi-)
+      static const DaughterPair AntiLambdaDaughters; ///< Anti-Lambda daughter particle types (pi+, p-)
     };
 
+    /**
+     * Container class for RecoTrack with genfit Track access.
+     * 
+     * Provides a wrapper around RecoTrack objects with convenient access to
+     * the underlying genfit Track and track representation. Handles track
+     * copying, fitting preparation, and hit information access.
+     */
     class TrackContainer {
       friend Track;
     public:
+      /**
+       * Constructor for track container.
+       * 
+       * @param recoTrack Pointer to the RecoTrack object to wrap
+       * @param pdgClosest PDG code of the closest mass hypothesis
+       * @param isOriginal Whether this is the original track (true) or a copy (false)
+       */
       TrackContainer(
           RecoTrack* recoTrack,
           const uint pdgClosest,
           const bool isOriginal = true
       );
 
+      /**
+       * Destructor handles track representation restoration.
+       */
       ~TrackContainer();
 
     private:
+      /**
+       * Prepare track for vertex fitting by selecting appropriate track representation.
+       * 
+       * @return True if preparation was successful
+       */
       bool prepareVertexFit();
+      
+      /**
+       * Fit the track with current settings.
+       * 
+       * @return True if fitting was successful
+       */
       bool fit();
+      
+      /**
+       * Get hit information objects for this track.
+       * 
+       * @return Vector of pointers to RecoHitInformation objects
+       */
       const std::vector<RecoHitInformation*> getHitInfos() const;
+      
+      /**
+       * Get measured state on plane from first hit.
+       * 
+       * @return Measured state on plane at first hit
+       */
       genfit::MeasuredStateOnPlane getMSOPfromFirstHit() const;
+      
+      /**
+       * Get measured state on plane from specific hit.
+       * 
+       * @param hitInfo Pointer to specific hit information
+       * @return Measured state on plane at specified hit
+       */
       genfit::MeasuredStateOnPlane getMSOPfromHit(const RecoHitInformation* hitInfo) const;
       
+      /**
+       * Create a copy of the track container.
+       * 
+       * @param trackContainer Original track container to copy
+       * @return Unique pointer to copied track container
+       */
       static std::unique_ptr<TrackContainer> copy(const TrackContainer &trackContainer);
       
-      const genfit::AbsTrackRep* m_trackRep = nullptr;
-      RecoTrack* m_recoTrack = nullptr;
-      genfit::Track &m_genfitTrack;
-      const uint m_OriginalCardinalRepID = 0;
-      const uint m_pdgClosest = 0;
-      const Const::ChargedStable m_particleClosest;
-      const bool m_isOriginal = true;
-      bool m_isSmudged = false;
+      const genfit::AbsTrackRep* m_trackRep = nullptr; ///< Pointer to selected track representation
+      RecoTrack* m_recoTrack = nullptr; ///< Pointer to wrapped RecoTrack
+      genfit::Track &m_genfitTrack; ///< Reference to underlying genfit Track
+      const uint m_OriginalCardinalRepID = 0; ///< Original cardinal representation ID
+      const uint m_pdgClosest = 0; ///< PDG code of closest mass hypothesis
+      const Const::ChargedStable m_particleClosest; ///< Particle type of closest mass hypothesis
+      const bool m_isOriginal = true; ///< Whether this is the original track or a copy
+      bool m_isSmudged = false; ///< Whether track representation has been modified
     };
 
+    /**
+     * Track wrapper class for V0 fitting.
+     * 
+     * Wraps Belle II Track objects and provides functionality for V0 fitting including
+     * hit consistency checks, inner hit removal, track refitting, and shared cluster detection.
+     * Handles both original tracks and copied tracks for hit removal algorithms.
+     */
     class Track {
       friend Fitter::Fitter(const Config&);
     public:  
+      /**
+       * Constructor for track wrapper.
+       * 
+       * @param isTrackPlus Whether this is the positive track in the V0 pair
+       * @param track Pointer to the Belle II Track object
+       * @param particleHyp Particle hypothesis for this track
+       */
       Track(
         const bool isTrackPlus,
         const Belle2::Track* track, 
         const Const::ChargedStable& particleHyp
       );
       
+      /**
+       * Prepare track for vertex fitting.
+       * 
+       * @return True if preparation was successful
+       */
       bool prepareVertexFit();
+      
+      /**
+       * Check for inner hits relative to vertex position.
+       * 
+       * @param vertexPos Position of the V0 vertex
+       * @return True if check completed successfully
+       */
       bool checkInnerHits(const ROOT::Math::XYZVector& vertexPos);
+      
+      /**
+       * Check consistency of hits on the track.
+       */
       void checkHitConsistency();
+      
+      /**
+       * Remove inner hits and refit the track.
+       * 
+       * @param vertexPos Position of the V0 vertex for determining inner hits
+       * @return True if hit removal and refitting was successful
+       */
       bool removeInnerHitsAndRefit(const ROOT::Math::XYZVector& vertexPos);
+      
+      /**
+       * Copy track and refit with current settings.
+       * 
+       * @return True if copying and refitting was successful
+       */
       bool copyAndRefit();
+      
+      /**
+       * Check if track has been refitted.
+       * 
+       * @return True if track was refitted
+       */
       bool isRefit() const;
+      
+      /**
+       * Check if track has inner hits.
+       * 
+       * @return True if track has inner hits
+       */
       bool hasInnerHit() const;
+      
+      /**
+       * Check if track has hit inconsistencies.
+       * 
+       * @return True if track has hit inconsistencies
+       */
       bool hasHitInconsistency() const;
+      
+      /**
+       * Get the underlying Belle II Track object.
+       * 
+       * @return Pointer to the Track object
+       */
       const Belle2::Track *getTrack() const;
+      
+      /**
+       * Get the underlying genfit Track object.
+       * 
+       * @return Pointer to the genfit Track
+       */
       genfit::Track *getGFTrack();
+      
+      /**
+       * Compute 4-momentum vector at vertex from track parameters.
+       * 
+       * @param trackParamsAtVertex Track parameters at the vertex
+       * @return 4-momentum vector with track mass hypothesis
+       */
       ROOT::Math::PxPyPzMVector computeLorentzVectorAtVertex(
         const genfit::GFRaveTrackParameters* trackParamsAtVertex
       ) const;
+      
+      /**
+       * Store track fit result in DataStore.
+       * 
+       * @param Bz Magnetic field strength at vertex
+       * @param sharedInnermostClusterFlag Flag indicating shared innermost clusters
+       * @return Pointer to stored TrackFitResult
+       */
       const TrackFitResult* storeTrackFitResult(
         const double Bz,
         const unsigned short sharedInnermostClusterFlag
       ) const;
+      
+      /**
+       * Check for shared innermost clusters between track pair.
+       * 
+       * @param trackPlus Positive track in the pair
+       * @param trackMinus Negative track in the pair
+       * @param sharedInnermostClusterFlag Output flag indicating shared clusters
+       * @return True if check completed successfully
+       */
       static bool checkSharedInnermostCluster(
         const Track& trackPlus, 
         const Track& trackMinus,
@@ -225,15 +569,15 @@
       );
 
   private:
-    const Const::ChargedStable m_particleHyp;
-    std::unique_ptr<TrackContainer> m_trackContainer;
-    const Belle2::Track* m_track;
-    uint m_nRemoved = 0;
-    const bool m_isTrackPlus = true;
-    bool m_hasHitInconsistency = false;
-    bool m_hasInnerHit = false;
+    const Const::ChargedStable m_particleHyp; ///< Particle hypothesis for this track
+    std::unique_ptr<TrackContainer> m_trackContainer; ///< Container for RecoTrack access
+    const Belle2::Track* m_track; ///< Pointer to Belle II Track object
+    uint m_nRemoved = 0; ///< Number of hits removed during processing
+    const bool m_isTrackPlus = true; ///< Whether this is the positive track in V0 pair
+    bool m_hasHitInconsistency = false; ///< Flag for hit inconsistencies
+    bool m_hasInnerHit = false; ///< Flag for presence of inner hits
     
-    static const Config* s_config;
+    static const Config* s_config; ///< Pointer to global configuration
     };
   }
 }
```

</details>

<details>

<summary>Squashed changes (full)</summary>

```diff
--- a/tracking/v0Finding/fitter/include/NewV0Fitter.h
+++ b/tracking/v0Finding/fitter/include/NewV0Fitter.h
@@ -1,240 +1,584 @@
 /**************************************************************************
  * basf2 (Belle II Analysis Software Framework)                           *
  * Author: The Belle II Collaboration                                     *
  *                                                                        *
  * See git log for contributors and copyright holders.                    *
  * This file is licensed under LGPL-3.0, see LICENSE.md.                  *
  **************************************************************************/
+
+/** @file NewV0Fitter.h
+ * New V0 finding and fitting algorithm implementation.
+ * 
+ * This file contains classes for fitting V0 vertices from track pairs,
+ * including track management, vertex fitting, and validation functionality.
+ */
+
 #pragma once
 
 #include <genfit/AbsTrackRep.h>
 #include <genfit/GFRaveTrackParameters.h>
 #include <genfit/GFRaveVertexFactory.h>
 #include <genfit/MeasuredStateOnPlane.h>
 #include <mdst/dataobjects/Track.h>
 #include <tracking/dataobjects/RecoHitInformation.h>
 #include <Math/Vector3Dfwd.h>
 #include <Math/Vector4Dfwd.h>
 #include <framework/datastore/StoreArray.h>
 #include <framework/gearbox/Const.h>
 #include <genfit/GFRaveVertex.h>
 #include <genfit/Track.h>
 #include <mdst/dataobjects/TrackFitResult.h>
 #include <mdst/dataobjects/V0.h>
 #include <tracking/dataobjects/RecoTrack.h>
 #include <tracking/dataobjects/V0ValidationVertex.h>
 #include <Math/Vector3D.h>
 
 #include <sys/types.h>
 #include <memory>
 #include <optional>
 #include <utility>
 #include <vector>
 
 namespace Belle2 {
+  /**
+   * Namespace containing the new V0 finding and fitting algorithm.
+   * 
+   * This namespace provides a complete framework for V0 particle reconstruction,
+   * including track fitting, vertex finding, and validation tools for particles
+   * like K_short, Lambda, and photon conversions.
+   */
   namespace NewV0Fitter {
 
+    /**
+     * Configuration parameters for the V0 fitter.
+     * 
+     * Contains all settings needed to configure the V0 finding algorithm,
+     * including fitting modes, validation options, mass ranges, and array names.
+     */
+    /**
+     * Configuration parameters for the V0 fitter.
+     * 
+     * Contains all settings needed to configure the V0 finding algorithm,
+     * including fitting modes, validation options, mass ranges, and array names.
+     */
     struct Config {
     public:
+      /**
+       * Constructor for V0 fitter configuration.
+       * 
+       * @param v0FitterMode Operating mode of the V0 fitter (0, 1, or 2)
+       * @param enableValidation Whether to enable validation vertex creation
+       * @param beamPipeRadius Radius of the beam pipe for geometric cuts
+       * @param vertexChi2CutOutside Chi2 cut for vertices outside beam pipe
+       * @param invMassRangeKshort Invariant mass range for K_short hypothesis
+       * @param invMassRangeLambda Invariant mass range for Lambda hypothesis  
+       * @param invMassRangePhoton Invariant mass range for photon hypothesis
+       * @param arrayNameTFResults Name of TrackFitResult array in DataStore
+       * @param arrayNameV0s Name of V0 array in DataStore
+       * @param arrayNameV0ValidationVertices Name of validation vertex array
+       * @param arrayNameRecoTracks Name of RecoTrack array in DataStore
+       * @param arrayNameCopiedRecoTracks Name of copied RecoTrack array
+       */
       Config(int v0FitterMode, bool enableValidation, double beamPipeRadius,
              double vertexChi2CutOutside,
              const std::tuple<double, double>& invMassRangeKshort,
              const std::tuple<double, double>& invMassRangeLambda,
              const std::tuple<double, double>& invMassRangePhoton,
              const std::string& arrayNameTFResults, const std::string& arrayNameV0s,
              const std::string& arrayNameV0ValidationVertices,
              const std::string& arrayNameRecoTracks,
              const std::string& arrayNameCopiedRecoTracks);
       
       
-      const std::string arrayNameTFResults;
-      const std::string arrayNameV0s;
-      const std::string arrayNameV0ValidationVertices;
-      const std::string arrayNameRecoTracks;
-      const std::string arrayNameCopiedRecoTracks;
-      
-      const std::tuple<double, double> invMassRangeKshort;
-      const std::tuple<double, double> invMassRangeLambda;
-      const std::tuple<double, double> invMassRangePhoton;
-      const double beamPipeRadius;
-      const double vertexChi2CutOutside;
-
-      const int v0FitterMode;
-      const bool forcestore;
-      const bool useOnlyOneSVDHitPair;
-      const bool enableValidation;
+      const std::string arrayNameTFResults; ///< Name of TrackFitResult array in DataStore
+      const std::string arrayNameV0s; ///< Name of V0 array in DataStore
+      const std::string arrayNameV0ValidationVertices; ///< Name of validation vertex array
+      const std::string arrayNameRecoTracks; ///< Name of RecoTrack array in DataStore
+      const std::string arrayNameCopiedRecoTracks; ///< Name of copied RecoTrack array
+      
+      const std::tuple<double, double> invMassRangeKshort; ///< Invariant mass range for K_short hypothesis
+      const std::tuple<double, double> invMassRangeLambda; ///< Invariant mass range for Lambda hypothesis
+      const std::tuple<double, double> invMassRangePhoton; ///< Invariant mass range for photon hypothesis
+      const double beamPipeRadius; ///< Radius of the beam pipe for geometric cuts
+      const double vertexChi2CutOutside; ///< Chi2 cut for vertices outside beam pipe
+
+      const int v0FitterMode; ///< Operating mode of the V0 fitter (0, 1, or 2)
+      const bool forcestore; ///< Whether to force storage of V0 candidates
+      const bool useOnlyOneSVDHitPair; ///< Whether to use only one SVD hit pair per track
+      const bool enableValidation; ///< Whether to enable validation vertex creation
     };
 
+    /**
+     * Storage management class for V0 fitter DataStore arrays.
+     * 
+     * Manages initialization and access to all DataStore arrays used by the V0 fitter,
+     * including track fit results, V0 objects, validation vertices, and track collections.
+     */
     class Storage {
     public:
+      /**
+       * Initialize all DataStore arrays based on configuration.
+       * 
+       * @param config Configuration object containing array names and settings
+       */
       static void init(const Config& config);
       
-      static StoreArray<TrackFitResult>* TFResults;
-      static StoreArray<Belle2::V0>* V0s;
-      static std::optional<StoreArray<V0ValidationVertex> *> V0ValidationVertices;
-      static const StoreArray<RecoTrack>* RecoTracks;
-      static StoreArray<RecoTrack>* CopiedRecoTracks;
+      static StoreArray<TrackFitResult>* TFResults; ///< Pointer to TrackFitResult array
+      static StoreArray<Belle2::V0>* V0s; ///< Pointer to V0 array
+      static std::optional<StoreArray<V0ValidationVertex> *> V0ValidationVertices; ///< Optional pointer to validation vertex array
+      static const StoreArray<RecoTrack>* RecoTracks; ///< Pointer to input RecoTrack array
+      static StoreArray<RecoTrack>* CopiedRecoTracks; ///< Pointer to copied RecoTrack array
 
     private:
-      static StoreArray<TrackFitResult> s_arrayTFResults;
-      static StoreArray<Belle2::V0> s_arrayV0s;
-      static StoreArray<V0ValidationVertex> s_arrayV0ValidationVertices;
-      static StoreArray<RecoTrack> s_arrayRecoTracks;
-      static StoreArray<RecoTrack> s_arrayCopiedRecoTracks;
+      static StoreArray<TrackFitResult> s_arrayTFResults; ///< Static TrackFitResult array instance
+      static StoreArray<Belle2::V0> s_arrayV0s; ///< Static V0 array instance
+      static StoreArray<V0ValidationVertex> s_arrayV0ValidationVertices; ///< Static validation vertex array instance
+      static StoreArray<RecoTrack> s_arrayRecoTracks; ///< Static input RecoTrack array instance
+      static StoreArray<RecoTrack> s_arrayCopiedRecoTracks; ///< Static copied RecoTrack array instance
     };
 
     class Track;
     class V0;
 
+    /**
+     * Main V0 vertex fitter class.
+     * 
+     * Handles the complete V0 fitting workflow including vertex fitting,
+     * track parameter extraction, and storage of results. Supports multiple
+     * V0 hypotheses (K_short, Lambda, photon conversions).
+     */
     class Fitter {
     public:
+      /**
+       * Constructor for V0 fitter.
+       * 
+       * @param config Configuration object with fitter settings
+       */
       Fitter(const Config& config);
 
+      /**
+       * Fit a V0 vertex from two tracks and store the result.
+       * 
+       * @param trackPlus Positive track for V0 fitting
+       * @param trackMinus Negative track for V0 fitting  
+       * @param v0Hypothesis Particle hypothesis for the V0 (K_short, Lambda, etc.)
+       * @param isForceStored Output flag indicating if V0 was force-stored
+       * @param isHitRemoved Output flag indicating if hits were removed during fitting
+       * @return True if fitting and storage was successful
+       */
       bool fitAndStore(const Belle2::Track* trackPlus,
                        const Belle2::Track* trackMinus,
                        const Const::ParticleType& v0Hypothesis, bool& isForceStored,
                        bool& isHitRemoved);
 
+      /**
+       * Get track particle hypotheses for a given V0 hypothesis.
+       * 
+       * @param v0Hypothesis V0 particle type (K_short, Lambda, photon)
+       * @return Pair of particle hypotheses for positive and negative tracks
+       */
       static std::pair<Const::ParticleType, Const::ParticleType>
       getTrackHypotheses(const Const::ParticleType& v0Hypothesis);
     
     private:
+      /**
+       * Fit vertex and apply pre-selection cuts.
+       * 
+       * @param v0 V0 object to fit
+       * @param trackPlus Positive track
+       * @param trackMinus Negative track
+       * @return True if vertex fitting and cuts passed
+       */
       bool fitVertexAndPrecut(V0& v0, Track& trackPlus, Track& trackMinus);
       
-      const Config m_config;
-      genfit::GFRaveVertexFactory m_GFRaveVertexFactory;
+      const Config m_config; ///< Configuration parameters
+      genfit::GFRaveVertexFactory m_GFRaveVertexFactory; ///< Vertex factory for fitting
     };
 
+    /**
+     * V0 particle candidate class.
+     * 
+     * Represents a V0 particle candidate with its decay products, vertex information,
+     * and associated validation data. Handles creation, fitting, and storage of V0 objects
+     * for different particle hypotheses (K_short, Lambda, photon conversions).
+     */
     class V0 {
       friend Fitter::Fitter(const Config& config);
     public:
+      /// Pair containing V0 object and optional validation vertex
       typedef std::pair <
         std::unique_ptr<Belle2::V0>,
         std::unique_ptr<Belle2::V0ValidationVertex>
       > V0ObjPair;
+      /// Pair of particle types for the two daughter tracks
       typedef std::pair<Const::ParticleType, Const::ParticleType> DaughterPair;
 
-      bool invMassCut(const uint tolerance = 0) const;      
+      /**
+       * Check if V0 candidate passes invariant mass cuts.
+       * 
+       * @param tolerance Additional tolerance for mass window (default: 0)
+       * @return True if candidate passes mass cuts
+       */
+      bool invMassCut(const uint tolerance = 0) const;
+      
+      /**
+       * Temporarily store V0 object pair for later processing.
+       * 
+       * @param v0ObjPair Pair of V0 object and validation vertex to stash
+       */
       void stash(V0ObjPair&& v0ObjPair);
+      
+      /**
+       * Retrieve and store previously stashed V0 object.
+       * 
+       * @return True if stashed object was successfully stored
+       */
       bool stashPop();
+      
+      /**
+       * Get daughter particle hypotheses for this V0.
+       * 
+       * @return Pair of particle types for positive and negative tracks
+       */
       const DaughterPair &getDaughterParticles() const;
+      
+      /**
+       * Get vertex position of the V0.
+       * 
+       * @return 3D position vector of the vertex
+       */
       const ROOT::Math::XYZVector& getVertexPos() const;
+      
+      /**
+       * Get the fitted vertex object.
+       * 
+       * @return Reference to the GFRave vertex
+       */
       const genfit::GFRaveVertex &getGFRaveVertex() const;
+      
+      /**
+       * Set the fitted vertex for this V0.
+       * 
+       * @param gfRaveVertex Fitted vertex object
+       */
       void setVertex(genfit::GFRaveVertex &gfRaveVertex);
+      
+      /**
+       * Set the 4-momentum of the V0 at the vertex.
+       * 
+       * @param lorentzVectorAtVertex 4-momentum vector at vertex
+       */
       void setLorentzVectorAtVertex(ROOT::Math::PxPyPzMVector &&lorentzVectorAtVertex);
       
+      /**
+       * Store V0 object pair in DataStore.
+       * 
+       * @param v0ObjPair Pair containing V0 and validation vertex objects
+       */
       static void store(V0ObjPair&& v0ObjPair);
+      
+      /**
+       * Build V0 and validation vertex objects from fitted data.
+       * 
+       * @param v0 V0 candidate with fitted vertex
+       * @param trackPlus Positive daughter track
+       * @param trackMinus Negative daughter track
+       * @return Pair of created V0 objects
+       */
       static V0ObjPair buildObj(
         const V0& v0, 
         const Track& trackPlus,
         const Track& trackMinus
       );
+      
+      /**
+       * Create V0 candidate for given particle hypothesis.
+       * 
+       * @param v0Hypothesis Particle type hypothesis (K_short, Lambda, photon)
+       * @return V0 candidate object
+       */
       static V0 create(const Const::ParticleType& v0Hypothesis);
+      
+      /**
+       * Get daughter particle hypotheses for given V0 hypothesis.
+       * 
+       * @param v0Hypothesis V0 particle type
+       * @return Pair of daughter particle hypotheses
+       */
       static const DaughterPair getDaughterParticles(const Const::ParticleType& v0Hypothesis);
       
     private:  
+      /**
+       * Temporary storage for V0 objects during processing.
+       * 
+       * Holds V0 and validation vertex objects that can be retrieved
+       * and stored later in the processing workflow.
+       */
       struct V0ObjStash {
-        std::unique_ptr<Belle2::V0> m_V0;
-        std::unique_ptr<V0ValidationVertex> m_ValidationV0;
+        std::unique_ptr<Belle2::V0> m_V0; ///< Stored V0 object
+        std::unique_ptr<V0ValidationVertex> m_ValidationV0; ///< Stored validation vertex
+        
+        /**
+         * Constructor from V0 object pair.
+         * 
+         * @param v0ObjPair Pair containing V0 and validation objects
+         */
         V0ObjStash(V0ObjPair&& v0ObjPair) : 
           m_V0(std::move(std::get<0>(v0ObjPair))),
           m_ValidationV0(std::move(std::get<1>(v0ObjPair)))
         {}
       };
       
+      /**
+       * Constructor for V0 candidate.
+       * 
+       * @param V0Particle Particle type of the V0
+       * @param V0InvMassRange Allowed invariant mass range
+       * @param daughterParticles Particle types of daughter tracks
+       */
       V0(const Const::ParticleType& V0Particle,
          const std::tuple<double, double>& V0InvMassRange,
          const DaughterPair& daughterParticles);
       
-      std::optional<V0ObjStash> m_v0ObjStash;
-      
-      const Const::ParticleType& m_V0Particle;
-      const std::tuple<double, double>& m_V0InvMassRange;
-      const DaughterPair& m_daughterParticles;
-      genfit::GFRaveVertex m_GFRaveVertex;
-      ROOT::Math::XYZVector m_vertexPos;
-      ROOT::Math::PxPyPzMVector m_lorentzVectorAtVertex;
-
-      static const Config* s_config;
-      static const DaughterPair KshortDaughters;
-      static const DaughterPair PhotonDaughters;
-      static const DaughterPair LambdaDaughters;
-      static const DaughterPair AntiLambdaDaughters;
+      std::optional<V0ObjStash> m_v0ObjStash; ///< Optional temporary storage for V0 objects
+      
+      const Const::ParticleType& m_V0Particle; ///< Particle type of this V0
+      const std::tuple<double, double>& m_V0InvMassRange; ///< Invariant mass range for this V0 type
+      const DaughterPair& m_daughterParticles; ///< Particle types of daughter tracks
+      genfit::GFRaveVertex m_GFRaveVertex; ///< Fitted vertex object
+      ROOT::Math::XYZVector m_vertexPos; ///< Position of the fitted vertex
+      ROOT::Math::PxPyPzMVector m_lorentzVectorAtVertex; ///< 4-momentum at vertex
+
+      static const Config* s_config; ///< Pointer to global configuration
+      static const DaughterPair KshortDaughters; ///< K_short daughter particle types (pi+, pi-)
+      static const DaughterPair PhotonDaughters; ///< Photon daughter particle types (e+, e-)
+      static const DaughterPair LambdaDaughters; ///< Lambda daughter particle types (p+, pi-)
+      static const DaughterPair AntiLambdaDaughters; ///< Anti-Lambda daughter particle types (pi+, p-)
     };
 
+    /**
+     * Container class for RecoTrack with genfit Track access.
+     * 
+     * Provides a wrapper around RecoTrack objects with convenient access to
+     * the underlying genfit Track and track representation. Handles track
+     * copying, fitting preparation, and hit information access.
+     */
     class TrackContainer {
       friend Track;
     public:
+      /**
+       * Constructor for track container.
+       * 
+       * @param recoTrack Pointer to the RecoTrack object to wrap
+       * @param pdgClosest PDG code of the closest mass hypothesis
+       * @param isOriginal Whether this is the original track (true) or a copy (false)
+       */
       TrackContainer(
           RecoTrack* recoTrack,
           const uint pdgClosest,
           const bool isOriginal = true
       );
 
+      /**
+       * Destructor handles track representation restoration.
+       */
       ~TrackContainer();
 
     private:
+      /**
+       * Prepare track for vertex fitting by selecting appropriate track representation.
+       * 
+       * @return True if preparation was successful
+       */
       bool prepareVertexFit();
+      
+      /**
+       * Fit the track with current settings.
+       * 
+       * @return True if fitting was successful
+       */
       bool fit();
+      
+      /**
+       * Get hit information objects for this track.
+       * 
+       * @return Vector of pointers to RecoHitInformation objects
+       */
       const std::vector<RecoHitInformation*> getHitInfos() const;
+      
+      /**
+       * Get measured state on plane from first hit.
+       * 
+       * @return Measured state on plane at first hit
+       */
       genfit::MeasuredStateOnPlane getMSOPfromFirstHit() const;
+      
+      /**
+       * Get measured state on plane from specific hit.
+       * 
+       * @param hitInfo Pointer to specific hit information
+       * @return Measured state on plane at specified hit
+       */
       genfit::MeasuredStateOnPlane getMSOPfromHit(const RecoHitInformation* hitInfo) const;
       
+      /**
+       * Create a copy of the track container.
+       * 
+       * @param trackContainer Original track container to copy
+       * @return Unique pointer to copied track container
+       */
       static std::unique_ptr<TrackContainer> copy(const TrackContainer &trackContainer);
       
-      const genfit::AbsTrackRep* m_trackRep = nullptr;
-      RecoTrack* m_recoTrack = nullptr;
-      genfit::Track &m_genfitTrack;
-      const uint m_OriginalCardinalRepID = 0;
-      const uint m_pdgClosest = 0;
-      const Const::ChargedStable m_particleClosest;
-      const bool m_isOriginal = true;
-      bool m_isSmudged = false;
+      const genfit::AbsTrackRep* m_trackRep = nullptr; ///< Pointer to selected track representation
+      RecoTrack* m_recoTrack = nullptr; ///< Pointer to wrapped RecoTrack
+      genfit::Track &m_genfitTrack; ///< Reference to underlying genfit Track
+      const uint m_OriginalCardinalRepID = 0; ///< Original cardinal representation ID
+      const uint m_pdgClosest = 0; ///< PDG code of closest mass hypothesis
+      const Const::ChargedStable m_particleClosest; ///< Particle type of closest mass hypothesis
+      const bool m_isOriginal = true; ///< Whether this is the original track or a copy
+      bool m_isSmudged = false; ///< Whether track representation has been modified
     };
 
+    /**
+     * Track wrapper class for V0 fitting.
+     * 
+     * Wraps Belle II Track objects and provides functionality for V0 fitting including
+     * hit consistency checks, inner hit removal, track refitting, and shared cluster detection.
+     * Handles both original tracks and copied tracks for hit removal algorithms.
+     */
     class Track {
       friend Fitter::Fitter(const Config&);
     public:  
+      /**
+       * Constructor for track wrapper.
+       * 
+       * @param isTrackPlus Whether this is the positive track in the V0 pair
+       * @param track Pointer to the Belle II Track object
+       * @param particleHyp Particle hypothesis for this track
+       */
       Track(
         const bool isTrackPlus,
         const Belle2::Track* track, 
         const Const::ChargedStable& particleHyp
       );
       
+      /**
+       * Prepare track for vertex fitting.
+       * 
+       * @return True if preparation was successful
+       */
       bool prepareVertexFit();
+      
+      /**
+       * Check for inner hits relative to vertex position.
+       * 
+       * @param vertexPos Position of the V0 vertex
+       * @return True if check completed successfully
+       */
       bool checkInnerHits(const ROOT::Math::XYZVector& vertexPos);
+      
+      /**
+       * Check consistency of hits on the track.
+       */
       void checkHitConsistency();
+      
+      /**
+       * Remove inner hits and refit the track.
+       * 
+       * @param vertexPos Position of the V0 vertex for determining inner hits
+       * @return True if hit removal and refitting was successful
+       */
       bool removeInnerHitsAndRefit(const ROOT::Math::XYZVector& vertexPos);
+      
+      /**
+       * Copy track and refit with current settings.
+       * 
+       * @return True if copying and refitting was successful
+       */
       bool copyAndRefit();
+      
+      /**
+       * Check if track has been refitted.
+       * 
+       * @return True if track was refitted
+       */
       bool isRefit() const;
+      
+      /**
+       * Check if track has inner hits.
+       * 
+       * @return True if track has inner hits
+       */
       bool hasInnerHit() const;
+      
+      /**
+       * Check if track has hit inconsistencies.
+       * 
+       * @return True if track has hit inconsistencies
+       */
       bool hasHitInconsistency() const;
+      
+      /**
+       * Get the underlying Belle II Track object.
+       * 
+       * @return Pointer to the Track object
+       */
       const Belle2::Track *getTrack() const;
+      
+      /**
+       * Get the underlying genfit Track object.
+       * 
+       * @return Pointer to the genfit Track
+       */
       genfit::Track *getGFTrack();
+      
+      /**
+       * Compute 4-momentum vector at vertex from track parameters.
+       * 
+       * @param trackParamsAtVertex Track parameters at the vertex
+       * @return 4-momentum vector with track mass hypothesis
+       */
       ROOT::Math::PxPyPzMVector computeLorentzVectorAtVertex(
         const genfit::GFRaveTrackParameters* trackParamsAtVertex
       ) const;
+      
+      /**
+       * Store track fit result in DataStore.
+       * 
+       * @param Bz Magnetic field strength at vertex
+       * @param sharedInnermostClusterFlag Flag indicating shared innermost clusters
+       * @return Pointer to stored TrackFitResult
+       */
       const TrackFitResult* storeTrackFitResult(
         const double Bz,
         const unsigned short sharedInnermostClusterFlag
       ) const;
+      
+      /**
+       * Check for shared innermost clusters between track pair.
+       * 
+       * @param trackPlus Positive track in the pair
+       * @param trackMinus Negative track in the pair
+       * @param sharedInnermostClusterFlag Output flag indicating shared clusters
+       * @return True if check completed successfully
+       */
       static bool checkSharedInnermostCluster(
         const Track& trackPlus, 
         const Track& trackMinus,
         unsigned short& sharedInnermostClusterFlag
       );
 
   private:
-    const Const::ChargedStable m_particleHyp;
-    std::unique_ptr<TrackContainer> m_trackContainer;
-    const Belle2::Track* m_track;
-    uint m_nRemoved = 0;
-    const bool m_isTrackPlus = true;
-    bool m_hasHitInconsistency = false;
-    bool m_hasInnerHit = false;
+    const Const::ChargedStable m_particleHyp; ///< Particle hypothesis for this track
+    std::unique_ptr<TrackContainer> m_trackContainer; ///< Container for RecoTrack access
+    const Belle2::Track* m_track; ///< Pointer to Belle II Track object
+    uint m_nRemoved = 0; ///< Number of hits removed during processing
+    const bool m_isTrackPlus = true; ///< Whether this is the positive track in V0 pair
+    bool m_hasHitInconsistency = false; ///< Flag for hit inconsistencies
+    bool m_hasInnerHit = false; ///< Flag for presence of inner hits
     
-    static const Config* s_config;
+    static const Config* s_config; ///< Pointer to global configuration
     };
   }
 }
 
```

</details>

</blockquote>

