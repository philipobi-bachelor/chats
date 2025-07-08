Document ID: `chat-logs/1500327`

<blockquote>

#### philipobi:
Add Doxygen-style documentation to #file:NewV0Fitter.h  Follow the conventions outlined in #file:doxygen-overview.md  and the relevant section of coding_conventions.rst. Refer to #file:NewV0Fitter.cc  for implementation details, but do not modify it. Keep the documentation concise, meaningful, and strictly comment-based—do not alter any code logic.

Variables: `file:NewV0Fitter.cc`, `file:doxygen-overview.md`, `file:NewV0Fitter.h`, `coding_conventions.rst`

</blockquote>

<blockquote>

#### GitHub Copilot (o4-mini):

<blockquote>

Edited `tracking/v0Finding/fitter/include/NewV0Fitter.h`

<details>

```diff
--- a/tracking/v0Finding/fitter/include/NewV0Fitter.h
+++ b/tracking/v0Finding/fitter/include/NewV0Fitter.h
@@ -5,6 +5,10 @@
  * See git log for contributors and copyright holders.                    *
  * This file is licensed under LGPL-3.0, see LICENSE.md.                  *
  **************************************************************************/
+/** @file NewV0Fitter.h
+ *  @brief Header defining classes and utilities for V0 vertex finding and fitting in Belle2.
+ */
+
 #pragma once
 
 #include <genfit/AbsTrackRep.h>
@@ -34,8 +38,31 @@
 namespace Belle2 {
   namespace NewV0Fitter {
 
+    /**
+     * @struct Config
+     * @brief Holds configuration parameters for the V0 fitter.
+     *
+     * Provides settings such as fitter mode, validation flags, beam pipe radius,
+     * vertex chi2 cuts, invariant mass ranges for different hypotheses,
+     * and input/output data store array names.
+     */
     struct Config {
     public:
+      /**
+       * @brief Construct a Config with full parameter specification.
+       * @param v0FitterMode Mode of operation (0=force store,1=standard,2=useOneSVDHitPair).
+       * @param enableValidation Enable creation of validation vertices.
+       * @param beamPipeRadius Radius of beam pipe for vertex fitting.
+       * @param vertexChi2CutOutside Chi2 cut threshold outside beam pipe.
+       * @param invMassRangeKshort Invariant mass range (min,max) for Kshort.
+       * @param invMassRangeLambda Invariant mass range (min,max) for Lambda.
+       * @param invMassRangePhoton Invariant mass range (min,max) for Photon.
+       * @param arrayNameTFResults Name of TrackFitResult array.
+       * @param arrayNameV0s Name of V0 array.
+       * @param arrayNameV0ValidationVertices Name of validation vertex array.
+       * @param arrayNameRecoTracks Name of input RecoTrack array.
+       * @param arrayNameCopiedRecoTracks Name of copied RecoTrack array.
+       */
       Config(int v0FitterMode, bool enableValidation, double beamPipeRadius,
              double vertexChi2CutOutside,
              const std::tuple<double, double>& invMassRangeKshort,
@@ -65,8 +92,16 @@
       const bool enableValidation;
     };
 
+    /**
+     * @class Storage
+     * @brief Manages registration and access to data store arrays used in V0 fitting.
+     */
     class Storage {
     public:
+      /**
+       * @brief Initialize all required data store arrays based on configuration.
+       * @param config Configuration specifying array names and validation options.
+       */
       static void init(const Config& config);
       
       static StoreArray<TrackFitResult>* TFResults;
@@ -83,18 +118,37 @@
       static StoreArray<RecoTrack> s_arrayCopiedRecoTracks;
     };
 
-    class Track;
-    class V0;
-
+    /**
+     * @class Fitter
+     * @brief Performs V0 candidate vertex finding, fitting, and storage.
+     */
     class Fitter {
     public:
+      /**
+       * @brief Construct a Fitter with provided configuration.
+       * @param config Configuration settings for fitting and storage.
+       */
       Fitter(const Config& config);
 
+      /**
+       * @brief Fit two tracks as a V0 candidate and store results.
+       * @param trackPlus Pointer to positive charged track.
+       * @param trackMinus Pointer to negative charged track.
+       * @param v0Hypothesis Particle type hypothesis of V0.
+       * @param isForceStored Output flag set if forced storage occurred.
+       * @param isHitRemoved Output flag set if inner hit removal occurred.
+       * @return True if fit and storage succeeded, false otherwise.
+       */
       bool fitAndStore(const Belle2::Track* trackPlus,
                        const Belle2::Track* trackMinus,
                        const Const::ParticleType& v0Hypothesis, bool& isForceStored,
                        bool& isHitRemoved);
 
+      /**
+       * @brief Determine daughter track hypotheses for a V0 type.
+       * @param v0Hypothesis V0 particle type.
+       * @return Pair of ChargedStable types for daughter tracks.
+       */
       static std::pair<Const::ParticleType, Const::ParticleType>
       getTrackHypotheses(const Const::ParticleType& v0Hypothesis);
     
@@ -105,6 +159,10 @@
       genfit::GFRaveVertexFactory m_GFRaveVertexFactory;
     };
 
+    /**
+     * @class V0
+     * @brief Represents a V0 candidate with vertex, 4-momentum, and validation.
+     */
     class V0 {
       friend Fitter::Fitter(const Config& config);
     public:
@@ -114,22 +172,86 @@
       > V0ObjPair;
       typedef std::pair<Const::ParticleType, Const::ParticleType> DaughterPair;
 
-      bool invMassCut(const uint tolerance = 0) const;      
+      /**
+       * @brief Check if the invariant mass is within the configured range.
+       * @param tolerance Allowed mass deviation (default 0).
+       * @return True if within range, false otherwise.
+       */
+      bool invMassCut(const uint tolerance = 0) const;
+
+      /**
+       * @brief Temporarily stash V0 object and its validation vertex.
+       * @param v0ObjPair Pair of unique pointers to V0 and its validation vertex.
+       */
       void stash(V0ObjPair&& v0ObjPair);
+
+      /**
+       * @brief Pop and permanently store previously stashed V0 objects.
+       * @return True if stash existed and storage succeeded.
+       */
       bool stashPop();
+
+      /**
+       * @brief Get the daughter particle types for this V0.
+       * @return Constant reference to DaughterPair of particle types.
+       */
       const DaughterPair &getDaughterParticles() const;
+
+      /**
+       * @brief Get the fitted vertex position.
+       * @return Constant reference to XYZVector representing vertex position.
+       */
       const ROOT::Math::XYZVector& getVertexPos() const;
+
+      /**
+       * @brief Get the GenFit Rave vertex object.
+       * @return Constant reference to GFRaveVertex.
+       */
       const genfit::GFRaveVertex &getGFRaveVertex() const;
+
+      /**
+       * @brief Set the vertex information from a GFRaveVertex.
+       * @param gfRaveVertex Reference to GFRaveVertex with vertex information.
+       */
       void setVertex(genfit::GFRaveVertex &gfRaveVertex);
+
+      /**
+       * @brief Set the Lorentz vector at the vertex for this V0.
+       * @param lorentzVectorAtVertex Rvalue reference to PxPyPzMVector.
+       */
       void setLorentzVectorAtVertex(ROOT::Math::PxPyPzMVector &&lorentzVectorAtVertex);
       
+      /**
+       * @brief Append V0 object pair to the data store.
+       * @param v0ObjPair Pair of unique pointers to V0 and validation vertex.
+       */
       static void store(V0ObjPair&& v0ObjPair);
+
+      /**
+       * @brief Build V0 and validation objects from fitted tracks.
+       * @param v0 Prototype V0 with configuration.
+       * @param trackPlus Fitter wrapper for positive track.
+       * @param trackMinus Fitter wrapper for negative track.
+       * @return Pair of unique pointers to new V0 and validation vertex.
+       */
       static V0ObjPair buildObj(
         const V0& v0, 
         const Track& trackPlus,
         const Track& trackMinus
       );
+
+      /**
+       * @brief Create a default V0 object for a hypothesis without fitting.
+       * @param v0Hypothesis Particle type hypothesis.
+       * @return V0 object initialized with mass range and daughter setting.
+       */
       static V0 create(const Const::ParticleType& v0Hypothesis);
+
+      /**
+       * @brief Get daughter particle types for specified V0 hypothesis.
+       * @param v0Hypothesis Particle type hypothesis.
+       * @return Pair of ChargedStable types for daughter tracks.
+       */
       static const DaughterPair getDaughterParticles(const Const::ParticleType& v0Hypothesis);
       
     private:  
@@ -162,15 +284,28 @@
       static const DaughterPair AntiLambdaDaughters;
     };
 
+    /**
+     * @class TrackContainer
+     * @brief Internal wrapper for handling RecoTrack and genfit::Track operations.
+     */
     class TrackContainer {
       friend Track;
     public:
+      /**
+       * @brief Construct TrackContainer wrapping a RecoTrack and PDG hypothesis.
+       * @param recoTrack Pointer to RecoTrack object.
+       * @param pdgClosest PDG code of closest mass hypothesis.
+       * @param isOriginal Flag indicating original track container.
+       */
       TrackContainer(
           RecoTrack* recoTrack,
           const uint pdgClosest,
           const bool isOriginal = true
       );
 
+      /**
+       * @brief Destructor resets smudge on original container if applied.
+       */
       ~TrackContainer();
 
     private:
@@ -192,49 +327,131 @@
       bool m_isSmudged = false;
     };
 
+    /**
+     * @class Track
+     * @brief Represents a charged track in the context of V0 fitting, with refit and hit checks.
+     */
     class Track {
       friend Fitter::Fitter(const Config&);
     public:  
+      /**
+       * @brief Construct a Track wrapper for V0 daughter fitting.
+       * @param isTrackPlus True if positive charged track.
+       * @param track Pointer to Belle2::Track data object.
+       * @param particleHyp Charged stable particle hypothesis.
+       */
       Track(
         const bool isTrackPlus,
         const Belle2::Track* track, 
         const Const::ChargedStable& particleHyp
       );
       
+      /**
+       * @brief Prepare the track for vertex fitting by selecting best GenFitter rep.
+       * @return True if preparation succeeded.
+       */
       bool prepareVertexFit();
+
+      /**
+       * @brief Check if this track has hits inside the fitted vertex position.
+       * @param vertexPos Decay vertex position.
+       * @return True if no errors, false otherwise.
+       */
       bool checkInnerHits(const ROOT::Math::XYZVector& vertexPos);
+
+      /**
+       * @brief Check consistency of hit ordering and detector layers.
+       */
       void checkHitConsistency();
+
+      /**
+       * @brief Remove inconsistent inner hits and refit the track.
+       * @param vertexPos Decay vertex position.
+       * @return True if refit succeeded, false otherwise.
+       */
       bool removeInnerHitsAndRefit(const ROOT::Math::XYZVector& vertexPos);
+
+      /**
+       * @brief Copy the track and refit, preserving original.
+       * @return True if copy and refit succeeded, false otherwise.
+       */
       bool copyAndRefit();
+
+      /**
+       * @brief Check if the track has been refitted.
+       * @return True if refitted, false otherwise.
+       */
       bool isRefit() const;
+
+      /**
+       * @brief Check if the track has any inner hits.
+       * @return True if inner hits exist, false otherwise.
+       */
       bool hasInnerHit() const;
+
+      /**
+       * @brief Check if there is any hit inconsistency.
+       * @return True if inconsistency detected, false otherwise.
+       */
       bool hasHitInconsistency() const;
+
+      /**
+       * @brief Get the underlying Belle2 track object.
+       * @return Pointer to constant Belle2::Track.
+       */
       const Belle2::Track *getTrack() const;
+
+      /**
+       * @brief Get the underlying GenFit track object.
+       * @return Pointer to mutable genfit::Track.
+       */
       genfit::Track *getGFTrack();
+
+      /**
+       * @brief Compute the Lorentz vector at the vertex from track parameters.
+       * @param trackParamsAtVertex Pointer to GenFit track parameters at vertex.
+       * @return Computed PxPyPzMVector representing 4-momentum at vertex.
+       */
       ROOT::Math::PxPyPzMVector computeLorentzVectorAtVertex(
         const genfit::GFRaveTrackParameters* trackParamsAtVertex
       ) const;
+
+      /**
+       * @brief Store the track fit result in the data store.
+       * @param Bz Magnetic field strength in Tesla.
+       * @param sharedInnermostClusterFlag Flag indicating shared innermost cluster.
+       * @return Pointer to the stored TrackFitResult.
+       */
       const TrackFitResult* storeTrackFitResult(
         const double Bz,
         const unsigned short sharedInnermostClusterFlag
       ) const;
+
+      /**
+       * @brief Check if two tracks share the same innermost cluster.
+       * @param trackPlus First track to compare.
+       * @param trackMinus Second track to compare.
+       * @param sharedInnermostClusterFlag Output flag indicating shared cluster.
+       * @return True if tracks share an innermost cluster, false otherwise.
+       */
       static bool checkSharedInnermostCluster(
         const Track& trackPlus, 
         const Track& trackMinus,
         unsigned short& sharedInnermostClusterFlag
       );
 
-  private:
-    const Const::ChargedStable m_particleHyp;
-    std::unique_ptr<TrackContainer> m_trackContainer;
-    const Belle2::Track* m_track;
-    uint m_nRemoved = 0;
-    const bool m_isTrackPlus = true;
-    bool m_hasHitInconsistency = false;
-    bool m_hasInnerHit = false;
+    private:
+      const Const::ChargedStable m_particleHyp;
+      std::unique_ptr<TrackContainer> m_trackContainer;
+      const Belle2::Track* m_track;
+      uint m_nRemoved = 0;
+      const bool m_isTrackPlus = true;
+      bool m_hasHitInconsistency = false;
+      bool m_hasInnerHit = false;
     
-    static const Config* s_config;
-    };
+      static const Config* s_config;
+    };
+
   }
 }
 
```

</details>

</blockquote>

I added Doxygen-style comment blocks in `NewV0Fitter.h` for file-level, classes, and public methods, following the provided conventions. Let me know if you’d like adjustments or additions.

`(2 min, 33.054 s)`

</blockquote>

<blockquote>

##### Edited Files:

`tracking/v0Finding/fitter/include/NewV0Fitter.h`:

<details>

<summary>Squashed changes (short)</summary>

```diff
--- a/tracking/v0Finding/fitter/include/NewV0Fitter.h
+++ b/tracking/v0Finding/fitter/include/NewV0Fitter.h
@@ -5,6 +5,10 @@
  * See git log for contributors and copyright holders.                    *
  * This file is licensed under LGPL-3.0, see LICENSE.md.                  *
  **************************************************************************/
+/** @file NewV0Fitter.h
+ *  @brief Header defining classes and utilities for V0 vertex finding and fitting in Belle2.
+ */
+
 #pragma once
 
 #include <genfit/AbsTrackRep.h>
@@ -34,8 +38,31 @@
 namespace Belle2 {
   namespace NewV0Fitter {
 
+    /**
+     * @struct Config
+     * @brief Holds configuration parameters for the V0 fitter.
+     *
+     * Provides settings such as fitter mode, validation flags, beam pipe radius,
+     * vertex chi2 cuts, invariant mass ranges for different hypotheses,
+     * and input/output data store array names.
+     */
     struct Config {
     public:
+      /**
+       * @brief Construct a Config with full parameter specification.
+       * @param v0FitterMode Mode of operation (0=force store,1=standard,2=useOneSVDHitPair).
+       * @param enableValidation Enable creation of validation vertices.
+       * @param beamPipeRadius Radius of beam pipe for vertex fitting.
+       * @param vertexChi2CutOutside Chi2 cut threshold outside beam pipe.
+       * @param invMassRangeKshort Invariant mass range (min,max) for Kshort.
+       * @param invMassRangeLambda Invariant mass range (min,max) for Lambda.
+       * @param invMassRangePhoton Invariant mass range (min,max) for Photon.
+       * @param arrayNameTFResults Name of TrackFitResult array.
+       * @param arrayNameV0s Name of V0 array.
+       * @param arrayNameV0ValidationVertices Name of validation vertex array.
+       * @param arrayNameRecoTracks Name of input RecoTrack array.
+       * @param arrayNameCopiedRecoTracks Name of copied RecoTrack array.
+       */
       Config(int v0FitterMode, bool enableValidation, double beamPipeRadius,
              double vertexChi2CutOutside,
              const std::tuple<double, double>& invMassRangeKshort,
@@ -65,8 +92,16 @@
       const bool enableValidation;
     };
 
+    /**
+     * @class Storage
+     * @brief Manages registration and access to data store arrays used in V0 fitting.
+     */
     class Storage {
     public:
+      /**
+       * @brief Initialize all required data store arrays based on configuration.
+       * @param config Configuration specifying array names and validation options.
+       */
       static void init(const Config& config);
       
       static StoreArray<TrackFitResult>* TFResults;
@@ -83,18 +118,37 @@
       static StoreArray<RecoTrack> s_arrayCopiedRecoTracks;
     };
 
-    class Track;
-    class V0;
-
+    /**
+     * @class Fitter
+     * @brief Performs V0 candidate vertex finding, fitting, and storage.
+     */
     class Fitter {
     public:
+      /**
+       * @brief Construct a Fitter with provided configuration.
+       * @param config Configuration settings for fitting and storage.
+       */
       Fitter(const Config& config);
 
+      /**
+       * @brief Fit two tracks as a V0 candidate and store results.
+       * @param trackPlus Pointer to positive charged track.
+       * @param trackMinus Pointer to negative charged track.
+       * @param v0Hypothesis Particle type hypothesis of V0.
+       * @param isForceStored Output flag set if forced storage occurred.
+       * @param isHitRemoved Output flag set if inner hit removal occurred.
+       * @return True if fit and storage succeeded, false otherwise.
+       */
       bool fitAndStore(const Belle2::Track* trackPlus,
                        const Belle2::Track* trackMinus,
                        const Const::ParticleType& v0Hypothesis, bool& isForceStored,
                        bool& isHitRemoved);
 
+      /**
+       * @brief Determine daughter track hypotheses for a V0 type.
+       * @param v0Hypothesis V0 particle type.
+       * @return Pair of ChargedStable types for daughter tracks.
+       */
       static std::pair<Const::ParticleType, Const::ParticleType>
       getTrackHypotheses(const Const::ParticleType& v0Hypothesis);
     
@@ -105,6 +159,10 @@
       genfit::GFRaveVertexFactory m_GFRaveVertexFactory;
     };
 
+    /**
+     * @class V0
+     * @brief Represents a V0 candidate with vertex, 4-momentum, and validation.
+     */
     class V0 {
       friend Fitter::Fitter(const Config& config);
     public:
@@ -114,22 +172,86 @@
       > V0ObjPair;
       typedef std::pair<Const::ParticleType, Const::ParticleType> DaughterPair;
 
-      bool invMassCut(const uint tolerance = 0) const;      
+      /**
+       * @brief Check if the invariant mass is within the configured range.
+       * @param tolerance Allowed mass deviation (default 0).
+       * @return True if within range, false otherwise.
+       */
+      bool invMassCut(const uint tolerance = 0) const;
+
+      /**
+       * @brief Temporarily stash V0 object and its validation vertex.
+       * @param v0ObjPair Pair of unique pointers to V0 and its validation vertex.
+       */
       void stash(V0ObjPair&& v0ObjPair);
+
+      /**
+       * @brief Pop and permanently store previously stashed V0 objects.
+       * @return True if stash existed and storage succeeded.
+       */
       bool stashPop();
+
+      /**
+       * @brief Get the daughter particle types for this V0.
+       * @return Constant reference to DaughterPair of particle types.
+       */
       const DaughterPair &getDaughterParticles() const;
+
+      /**
+       * @brief Get the fitted vertex position.
+       * @return Constant reference to XYZVector representing vertex position.
+       */
       const ROOT::Math::XYZVector& getVertexPos() const;
+
+      /**
+       * @brief Get the GenFit Rave vertex object.
+       * @return Constant reference to GFRaveVertex.
+       */
       const genfit::GFRaveVertex &getGFRaveVertex() const;
+
+      /**
+       * @brief Set the vertex information from a GFRaveVertex.
+       * @param gfRaveVertex Reference to GFRaveVertex with vertex information.
+       */
       void setVertex(genfit::GFRaveVertex &gfRaveVertex);
+
+      /**
+       * @brief Set the Lorentz vector at the vertex for this V0.
+       * @param lorentzVectorAtVertex Rvalue reference to PxPyPzMVector.
+       */
       void setLorentzVectorAtVertex(ROOT::Math::PxPyPzMVector &&lorentzVectorAtVertex);
       
+      /**
+       * @brief Append V0 object pair to the data store.
+       * @param v0ObjPair Pair of unique pointers to V0 and validation vertex.
+       */
       static void store(V0ObjPair&& v0ObjPair);
+
+      /**
+       * @brief Build V0 and validation objects from fitted tracks.
+       * @param v0 Prototype V0 with configuration.
+       * @param trackPlus Fitter wrapper for positive track.
+       * @param trackMinus Fitter wrapper for negative track.
+       * @return Pair of unique pointers to new V0 and validation vertex.
+       */
       static V0ObjPair buildObj(
         const V0& v0, 
         const Track& trackPlus,
         const Track& trackMinus
       );
+
+      /**
+       * @brief Create a default V0 object for a hypothesis without fitting.
+       * @param v0Hypothesis Particle type hypothesis.
+       * @return V0 object initialized with mass range and daughter setting.
+       */
       static V0 create(const Const::ParticleType& v0Hypothesis);
+
+      /**
+       * @brief Get daughter particle types for specified V0 hypothesis.
+       * @param v0Hypothesis Particle type hypothesis.
+       * @return Pair of ChargedStable types for daughter tracks.
+       */
       static const DaughterPair getDaughterParticles(const Const::ParticleType& v0Hypothesis);
       
     private:  
@@ -162,15 +284,28 @@
       static const DaughterPair AntiLambdaDaughters;
     };
 
+    /**
+     * @class TrackContainer
+     * @brief Internal wrapper for handling RecoTrack and genfit::Track operations.
+     */
     class TrackContainer {
       friend Track;
     public:
+      /**
+       * @brief Construct TrackContainer wrapping a RecoTrack and PDG hypothesis.
+       * @param recoTrack Pointer to RecoTrack object.
+       * @param pdgClosest PDG code of closest mass hypothesis.
+       * @param isOriginal Flag indicating original track container.
+       */
       TrackContainer(
           RecoTrack* recoTrack,
           const uint pdgClosest,
           const bool isOriginal = true
       );
 
+      /**
+       * @brief Destructor resets smudge on original container if applied.
+       */
       ~TrackContainer();
 
     private:
@@ -192,49 +327,131 @@
       bool m_isSmudged = false;
     };
 
+    /**
+     * @class Track
+     * @brief Represents a charged track in the context of V0 fitting, with refit and hit checks.
+     */
     class Track {
       friend Fitter::Fitter(const Config&);
     public:  
+      /**
+       * @brief Construct a Track wrapper for V0 daughter fitting.
+       * @param isTrackPlus True if positive charged track.
+       * @param track Pointer to Belle2::Track data object.
+       * @param particleHyp Charged stable particle hypothesis.
+       */
       Track(
         const bool isTrackPlus,
         const Belle2::Track* track, 
         const Const::ChargedStable& particleHyp
       );
       
+      /**
+       * @brief Prepare the track for vertex fitting by selecting best GenFitter rep.
+       * @return True if preparation succeeded.
+       */
       bool prepareVertexFit();
+
+      /**
+       * @brief Check if this track has hits inside the fitted vertex position.
+       * @param vertexPos Decay vertex position.
+       * @return True if no errors, false otherwise.
+       */
       bool checkInnerHits(const ROOT::Math::XYZVector& vertexPos);
+
+      /**
+       * @brief Check consistency of hit ordering and detector layers.
+       */
       void checkHitConsistency();
+
+      /**
+       * @brief Remove inconsistent inner hits and refit the track.
+       * @param vertexPos Decay vertex position.
+       * @return True if refit succeeded, false otherwise.
+       */
       bool removeInnerHitsAndRefit(const ROOT::Math::XYZVector& vertexPos);
+
+      /**
+       * @brief Copy the track and refit, preserving original.
+       * @return True if copy and refit succeeded, false otherwise.
+       */
       bool copyAndRefit();
+
+      /**
+       * @brief Check if the track has been refitted.
+       * @return True if refitted, false otherwise.
+       */
       bool isRefit() const;
+
+      /**
+       * @brief Check if the track has any inner hits.
+       * @return True if inner hits exist, false otherwise.
+       */
       bool hasInnerHit() const;
+
+      /**
+       * @brief Check if there is any hit inconsistency.
+       * @return True if inconsistency detected, false otherwise.
+       */
       bool hasHitInconsistency() const;
+
+      /**
+       * @brief Get the underlying Belle2 track object.
+       * @return Pointer to constant Belle2::Track.
+       */
       const Belle2::Track *getTrack() const;
+
+      /**
+       * @brief Get the underlying GenFit track object.
+       * @return Pointer to mutable genfit::Track.
+       */
       genfit::Track *getGFTrack();
+
+      /**
+       * @brief Compute the Lorentz vector at the vertex from track parameters.
+       * @param trackParamsAtVertex Pointer to GenFit track parameters at vertex.
+       * @return Computed PxPyPzMVector representing 4-momentum at vertex.
+       */
       ROOT::Math::PxPyPzMVector computeLorentzVectorAtVertex(
         const genfit::GFRaveTrackParameters* trackParamsAtVertex
       ) const;
+
+      /**
+       * @brief Store the track fit result in the data store.
+       * @param Bz Magnetic field strength in Tesla.
+       * @param sharedInnermostClusterFlag Flag indicating shared innermost cluster.
+       * @return Pointer to the stored TrackFitResult.
+       */
       const TrackFitResult* storeTrackFitResult(
         const double Bz,
         const unsigned short sharedInnermostClusterFlag
       ) const;
+
+      /**
+       * @brief Check if two tracks share the same innermost cluster.
+       * @param trackPlus First track to compare.
+       * @param trackMinus Second track to compare.
+       * @param sharedInnermostClusterFlag Output flag indicating shared cluster.
+       * @return True if tracks share an innermost cluster, false otherwise.
+       */
       static bool checkSharedInnermostCluster(
         const Track& trackPlus, 
         const Track& trackMinus,
         unsigned short& sharedInnermostClusterFlag
       );
 
-  private:
-    const Const::ChargedStable m_particleHyp;
-    std::unique_ptr<TrackContainer> m_trackContainer;
-    const Belle2::Track* m_track;
-    uint m_nRemoved = 0;
-    const bool m_isTrackPlus = true;
-    bool m_hasHitInconsistency = false;
-    bool m_hasInnerHit = false;
+    private:
+      const Const::ChargedStable m_particleHyp;
+      std::unique_ptr<TrackContainer> m_trackContainer;
+      const Belle2::Track* m_track;
+      uint m_nRemoved = 0;
+      const bool m_isTrackPlus = true;
+      bool m_hasHitInconsistency = false;
+      bool m_hasInnerHit = false;
     
-    static const Config* s_config;
-    };
+      static const Config* s_config;
+    };
+
   }
 }
 
```

</details>

<details>

<summary>Squashed changes (full)</summary>

```diff
--- a/tracking/v0Finding/fitter/include/NewV0Fitter.h
+++ b/tracking/v0Finding/fitter/include/NewV0Fitter.h
@@ -1,240 +1,457 @@
 /**************************************************************************
  * basf2 (Belle II Analysis Software Framework)                           *
  * Author: The Belle II Collaboration                                     *
  *                                                                        *
  * See git log for contributors and copyright holders.                    *
  * This file is licensed under LGPL-3.0, see LICENSE.md.                  *
  **************************************************************************/
+/** @file NewV0Fitter.h
+ *  @brief Header defining classes and utilities for V0 vertex finding and fitting in Belle2.
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
   namespace NewV0Fitter {
 
+    /**
+     * @struct Config
+     * @brief Holds configuration parameters for the V0 fitter.
+     *
+     * Provides settings such as fitter mode, validation flags, beam pipe radius,
+     * vertex chi2 cuts, invariant mass ranges for different hypotheses,
+     * and input/output data store array names.
+     */
     struct Config {
     public:
+      /**
+       * @brief Construct a Config with full parameter specification.
+       * @param v0FitterMode Mode of operation (0=force store,1=standard,2=useOneSVDHitPair).
+       * @param enableValidation Enable creation of validation vertices.
+       * @param beamPipeRadius Radius of beam pipe for vertex fitting.
+       * @param vertexChi2CutOutside Chi2 cut threshold outside beam pipe.
+       * @param invMassRangeKshort Invariant mass range (min,max) for Kshort.
+       * @param invMassRangeLambda Invariant mass range (min,max) for Lambda.
+       * @param invMassRangePhoton Invariant mass range (min,max) for Photon.
+       * @param arrayNameTFResults Name of TrackFitResult array.
+       * @param arrayNameV0s Name of V0 array.
+       * @param arrayNameV0ValidationVertices Name of validation vertex array.
+       * @param arrayNameRecoTracks Name of input RecoTrack array.
+       * @param arrayNameCopiedRecoTracks Name of copied RecoTrack array.
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
       
       
       const std::string arrayNameTFResults;
       const std::string arrayNameV0s;
       const std::string arrayNameV0ValidationVertices;
       const std::string arrayNameRecoTracks;
       const std::string arrayNameCopiedRecoTracks;
       
       const std::tuple<double, double> invMassRangeKshort;
       const std::tuple<double, double> invMassRangeLambda;
       const std::tuple<double, double> invMassRangePhoton;
       const double beamPipeRadius;
       const double vertexChi2CutOutside;
 
       const int v0FitterMode;
       const bool forcestore;
       const bool useOnlyOneSVDHitPair;
       const bool enableValidation;
     };
 
+    /**
+     * @class Storage
+     * @brief Manages registration and access to data store arrays used in V0 fitting.
+     */
     class Storage {
     public:
+      /**
+       * @brief Initialize all required data store arrays based on configuration.
+       * @param config Configuration specifying array names and validation options.
+       */
       static void init(const Config& config);
       
       static StoreArray<TrackFitResult>* TFResults;
       static StoreArray<Belle2::V0>* V0s;
       static std::optional<StoreArray<V0ValidationVertex> *> V0ValidationVertices;
       static const StoreArray<RecoTrack>* RecoTracks;
       static StoreArray<RecoTrack>* CopiedRecoTracks;
 
     private:
       static StoreArray<TrackFitResult> s_arrayTFResults;
       static StoreArray<Belle2::V0> s_arrayV0s;
       static StoreArray<V0ValidationVertex> s_arrayV0ValidationVertices;
       static StoreArray<RecoTrack> s_arrayRecoTracks;
       static StoreArray<RecoTrack> s_arrayCopiedRecoTracks;
     };
 
-    class Track;
-    class V0;
-
+    /**
+     * @class Fitter
+     * @brief Performs V0 candidate vertex finding, fitting, and storage.
+     */
     class Fitter {
     public:
+      /**
+       * @brief Construct a Fitter with provided configuration.
+       * @param config Configuration settings for fitting and storage.
+       */
       Fitter(const Config& config);
 
+      /**
+       * @brief Fit two tracks as a V0 candidate and store results.
+       * @param trackPlus Pointer to positive charged track.
+       * @param trackMinus Pointer to negative charged track.
+       * @param v0Hypothesis Particle type hypothesis of V0.
+       * @param isForceStored Output flag set if forced storage occurred.
+       * @param isHitRemoved Output flag set if inner hit removal occurred.
+       * @return True if fit and storage succeeded, false otherwise.
+       */
       bool fitAndStore(const Belle2::Track* trackPlus,
                        const Belle2::Track* trackMinus,
                        const Const::ParticleType& v0Hypothesis, bool& isForceStored,
                        bool& isHitRemoved);
 
+      /**
+       * @brief Determine daughter track hypotheses for a V0 type.
+       * @param v0Hypothesis V0 particle type.
+       * @return Pair of ChargedStable types for daughter tracks.
+       */
       static std::pair<Const::ParticleType, Const::ParticleType>
       getTrackHypotheses(const Const::ParticleType& v0Hypothesis);
     
     private:
       bool fitVertexAndPrecut(V0& v0, Track& trackPlus, Track& trackMinus);
       
       const Config m_config;
       genfit::GFRaveVertexFactory m_GFRaveVertexFactory;
     };
 
+    /**
+     * @class V0
+     * @brief Represents a V0 candidate with vertex, 4-momentum, and validation.
+     */
     class V0 {
       friend Fitter::Fitter(const Config& config);
     public:
       typedef std::pair <
         std::unique_ptr<Belle2::V0>,
         std::unique_ptr<Belle2::V0ValidationVertex>
       > V0ObjPair;
       typedef std::pair<Const::ParticleType, Const::ParticleType> DaughterPair;
 
-      bool invMassCut(const uint tolerance = 0) const;      
+      /**
+       * @brief Check if the invariant mass is within the configured range.
+       * @param tolerance Allowed mass deviation (default 0).
+       * @return True if within range, false otherwise.
+       */
+      bool invMassCut(const uint tolerance = 0) const;
+
+      /**
+       * @brief Temporarily stash V0 object and its validation vertex.
+       * @param v0ObjPair Pair of unique pointers to V0 and its validation vertex.
+       */
       void stash(V0ObjPair&& v0ObjPair);
+
+      /**
+       * @brief Pop and permanently store previously stashed V0 objects.
+       * @return True if stash existed and storage succeeded.
+       */
       bool stashPop();
+
+      /**
+       * @brief Get the daughter particle types for this V0.
+       * @return Constant reference to DaughterPair of particle types.
+       */
       const DaughterPair &getDaughterParticles() const;
+
+      /**
+       * @brief Get the fitted vertex position.
+       * @return Constant reference to XYZVector representing vertex position.
+       */
       const ROOT::Math::XYZVector& getVertexPos() const;
+
+      /**
+       * @brief Get the GenFit Rave vertex object.
+       * @return Constant reference to GFRaveVertex.
+       */
       const genfit::GFRaveVertex &getGFRaveVertex() const;
+
+      /**
+       * @brief Set the vertex information from a GFRaveVertex.
+       * @param gfRaveVertex Reference to GFRaveVertex with vertex information.
+       */
       void setVertex(genfit::GFRaveVertex &gfRaveVertex);
+
+      /**
+       * @brief Set the Lorentz vector at the vertex for this V0.
+       * @param lorentzVectorAtVertex Rvalue reference to PxPyPzMVector.
+       */
       void setLorentzVectorAtVertex(ROOT::Math::PxPyPzMVector &&lorentzVectorAtVertex);
       
+      /**
+       * @brief Append V0 object pair to the data store.
+       * @param v0ObjPair Pair of unique pointers to V0 and validation vertex.
+       */
       static void store(V0ObjPair&& v0ObjPair);
+
+      /**
+       * @brief Build V0 and validation objects from fitted tracks.
+       * @param v0 Prototype V0 with configuration.
+       * @param trackPlus Fitter wrapper for positive track.
+       * @param trackMinus Fitter wrapper for negative track.
+       * @return Pair of unique pointers to new V0 and validation vertex.
+       */
       static V0ObjPair buildObj(
         const V0& v0, 
         const Track& trackPlus,
         const Track& trackMinus
       );
+
+      /**
+       * @brief Create a default V0 object for a hypothesis without fitting.
+       * @param v0Hypothesis Particle type hypothesis.
+       * @return V0 object initialized with mass range and daughter setting.
+       */
       static V0 create(const Const::ParticleType& v0Hypothesis);
+
+      /**
+       * @brief Get daughter particle types for specified V0 hypothesis.
+       * @param v0Hypothesis Particle type hypothesis.
+       * @return Pair of ChargedStable types for daughter tracks.
+       */
       static const DaughterPair getDaughterParticles(const Const::ParticleType& v0Hypothesis);
       
     private:  
       struct V0ObjStash {
         std::unique_ptr<Belle2::V0> m_V0;
         std::unique_ptr<V0ValidationVertex> m_ValidationV0;
         V0ObjStash(V0ObjPair&& v0ObjPair) : 
           m_V0(std::move(std::get<0>(v0ObjPair))),
           m_ValidationV0(std::move(std::get<1>(v0ObjPair)))
         {}
       };
       
       V0(const Const::ParticleType& V0Particle,
          const std::tuple<double, double>& V0InvMassRange,
          const DaughterPair& daughterParticles);
       
       std::optional<V0ObjStash> m_v0ObjStash;
       
       const Const::ParticleType& m_V0Particle;
       const std::tuple<double, double>& m_V0InvMassRange;
       const DaughterPair& m_daughterParticles;
       genfit::GFRaveVertex m_GFRaveVertex;
       ROOT::Math::XYZVector m_vertexPos;
       ROOT::Math::PxPyPzMVector m_lorentzVectorAtVertex;
 
       static const Config* s_config;
       static const DaughterPair KshortDaughters;
       static const DaughterPair PhotonDaughters;
       static const DaughterPair LambdaDaughters;
       static const DaughterPair AntiLambdaDaughters;
     };
 
+    /**
+     * @class TrackContainer
+     * @brief Internal wrapper for handling RecoTrack and genfit::Track operations.
+     */
     class TrackContainer {
       friend Track;
     public:
+      /**
+       * @brief Construct TrackContainer wrapping a RecoTrack and PDG hypothesis.
+       * @param recoTrack Pointer to RecoTrack object.
+       * @param pdgClosest PDG code of closest mass hypothesis.
+       * @param isOriginal Flag indicating original track container.
+       */
       TrackContainer(
           RecoTrack* recoTrack,
           const uint pdgClosest,
           const bool isOriginal = true
       );
 
+      /**
+       * @brief Destructor resets smudge on original container if applied.
+       */
       ~TrackContainer();
 
     private:
       bool prepareVertexFit();
       bool fit();
       const std::vector<RecoHitInformation*> getHitInfos() const;
       genfit::MeasuredStateOnPlane getMSOPfromFirstHit() const;
       genfit::MeasuredStateOnPlane getMSOPfromHit(const RecoHitInformation* hitInfo) const;
       
       static std::unique_ptr<TrackContainer> copy(const TrackContainer &trackContainer);
       
       const genfit::AbsTrackRep* m_trackRep = nullptr;
       RecoTrack* m_recoTrack = nullptr;
       genfit::Track &m_genfitTrack;
       const uint m_OriginalCardinalRepID = 0;
       const uint m_pdgClosest = 0;
       const Const::ChargedStable m_particleClosest;
       const bool m_isOriginal = true;
       bool m_isSmudged = false;
     };
 
+    /**
+     * @class Track
+     * @brief Represents a charged track in the context of V0 fitting, with refit and hit checks.
+     */
     class Track {
       friend Fitter::Fitter(const Config&);
     public:  
+      /**
+       * @brief Construct a Track wrapper for V0 daughter fitting.
+       * @param isTrackPlus True if positive charged track.
+       * @param track Pointer to Belle2::Track data object.
+       * @param particleHyp Charged stable particle hypothesis.
+       */
       Track(
         const bool isTrackPlus,
         const Belle2::Track* track, 
         const Const::ChargedStable& particleHyp
       );
       
+      /**
+       * @brief Prepare the track for vertex fitting by selecting best GenFitter rep.
+       * @return True if preparation succeeded.
+       */
       bool prepareVertexFit();
+
+      /**
+       * @brief Check if this track has hits inside the fitted vertex position.
+       * @param vertexPos Decay vertex position.
+       * @return True if no errors, false otherwise.
+       */
       bool checkInnerHits(const ROOT::Math::XYZVector& vertexPos);
+
+      /**
+       * @brief Check consistency of hit ordering and detector layers.
+       */
       void checkHitConsistency();
+
+      /**
+       * @brief Remove inconsistent inner hits and refit the track.
+       * @param vertexPos Decay vertex position.
+       * @return True if refit succeeded, false otherwise.
+       */
       bool removeInnerHitsAndRefit(const ROOT::Math::XYZVector& vertexPos);
+
+      /**
+       * @brief Copy the track and refit, preserving original.
+       * @return True if copy and refit succeeded, false otherwise.
+       */
       bool copyAndRefit();
+
+      /**
+       * @brief Check if the track has been refitted.
+       * @return True if refitted, false otherwise.
+       */
       bool isRefit() const;
+
+      /**
+       * @brief Check if the track has any inner hits.
+       * @return True if inner hits exist, false otherwise.
+       */
       bool hasInnerHit() const;
+
+      /**
+       * @brief Check if there is any hit inconsistency.
+       * @return True if inconsistency detected, false otherwise.
+       */
       bool hasHitInconsistency() const;
+
+      /**
+       * @brief Get the underlying Belle2 track object.
+       * @return Pointer to constant Belle2::Track.
+       */
       const Belle2::Track *getTrack() const;
+
+      /**
+       * @brief Get the underlying GenFit track object.
+       * @return Pointer to mutable genfit::Track.
+       */
       genfit::Track *getGFTrack();
+
+      /**
+       * @brief Compute the Lorentz vector at the vertex from track parameters.
+       * @param trackParamsAtVertex Pointer to GenFit track parameters at vertex.
+       * @return Computed PxPyPzMVector representing 4-momentum at vertex.
+       */
       ROOT::Math::PxPyPzMVector computeLorentzVectorAtVertex(
         const genfit::GFRaveTrackParameters* trackParamsAtVertex
       ) const;
+
+      /**
+       * @brief Store the track fit result in the data store.
+       * @param Bz Magnetic field strength in Tesla.
+       * @param sharedInnermostClusterFlag Flag indicating shared innermost cluster.
+       * @return Pointer to the stored TrackFitResult.
+       */
       const TrackFitResult* storeTrackFitResult(
         const double Bz,
         const unsigned short sharedInnermostClusterFlag
       ) const;
+
+      /**
+       * @brief Check if two tracks share the same innermost cluster.
+       * @param trackPlus First track to compare.
+       * @param trackMinus Second track to compare.
+       * @param sharedInnermostClusterFlag Output flag indicating shared cluster.
+       * @return True if tracks share an innermost cluster, false otherwise.
+       */
       static bool checkSharedInnermostCluster(
         const Track& trackPlus, 
         const Track& trackMinus,
         unsigned short& sharedInnermostClusterFlag
       );
 
-  private:
-    const Const::ChargedStable m_particleHyp;
-    std::unique_ptr<TrackContainer> m_trackContainer;
-    const Belle2::Track* m_track;
-    uint m_nRemoved = 0;
-    const bool m_isTrackPlus = true;
-    bool m_hasHitInconsistency = false;
-    bool m_hasInnerHit = false;
+    private:
+      const Const::ChargedStable m_particleHyp;
+      std::unique_ptr<TrackContainer> m_trackContainer;
+      const Belle2::Track* m_track;
+      uint m_nRemoved = 0;
+      const bool m_isTrackPlus = true;
+      bool m_hasHitInconsistency = false;
+      bool m_hasInnerHit = false;
     
-    static const Config* s_config;
-    };
+      static const Config* s_config;
+    };
+
   }
 }
 
```

</details>

</blockquote>

