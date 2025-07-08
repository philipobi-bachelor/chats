Document ID: `chat-logs/1500790`

<blockquote>

#### philipobi:
Add Doxygen-style documentation to #file:NewV0Fitter.h  Follow the conventions outlined in #file:doxygen-overview.md  and the relevant section of coding_conventions.rst. Refer to #file:NewV0Fitter.cc  for implementation details, but do not modify it. Keep the documentation concise, meaningful, and strictly comment-based—do not alter any code logic.

Variables: `file:NewV0Fitter.cc`, `file:doxygen-overview.md`, `file:NewV0Fitter.h`, `coding_conventions.rst`

</blockquote>

<blockquote>

#### GitHub Copilot (gpt-4o):

<blockquote>

Edited `tracking/v0Finding/fitter/include/NewV0Fitter.h`

<details>

```diff
--- a/tracking/v0Finding/fitter/include/NewV0Fitter.h
+++ b/tracking/v0Finding/fitter/include/NewV0Fitter.h
@@ -31,11 +31,42 @@
 #include <utility>
 #include <vector>
 
+/**
+ * @file NewV0Fitter.h
+ * @brief Header file for the NewV0Fitter module in the Belle II framework.
+ *
+ * This file contains the declarations for the NewV0Fitter module, including
+ * configuration, storage, and fitting functionalities for V0 particles.
+ * It provides tools for vertex fitting, track handling, and validation.
+ */
+
 namespace Belle2 {
   namespace NewV0Fitter {
 
+    /**
+     * @class Config
+     * @brief Configuration settings for the NewV0Fitter module.
+     *
+     * This class encapsulates various parameters required for V0 fitting,
+     * including mass ranges, validation options, and array names.
+     */
     struct Config {
     public:
+      /**
+       * @brief Constructor for Config.
+       * @param v0FitterMode Mode of the V0 fitter.
+       * @param enableValidation Flag to enable validation.
+       * @param beamPipeRadius Radius of the beam pipe.
+       * @param vertexChi2CutOutside Chi-squared cut for vertices outside.
+       * @param invMassRangeKshort Mass range for Kshort particles.
+       * @param invMassRangeLambda Mass range for Lambda particles.
+       * @param invMassRangePhoton Mass range for Photon particles.
+       * @param arrayNameTFResults Name of the TrackFitResults array.
+       * @param arrayNameV0s Name of the V0s array.
+       * @param arrayNameV0ValidationVertices Name of the V0ValidationVertices array.
+       * @param arrayNameRecoTracks Name of the RecoTracks array.
+       * @param arrayNameCopiedRecoTracks Name of the CopiedRecoTracks array.
+       */
       Config(int v0FitterMode, bool enableValidation, double beamPipeRadius,
              double vertexChi2CutOutside,
              const std::tuple<double, double>& invMassRangeKshort,
@@ -65,8 +96,19 @@
       const bool enableValidation;
     };
 
+    /**
+     * @class Storage
+     * @brief Handles storage arrays for V0 fitting.
+     *
+     * This class manages arrays for storing TrackFitResults, V0s, validation vertices,
+     * and reconstructed tracks.
+     */
     class Storage {
     public:
+      /**
+       * @brief Initializes storage arrays based on the provided configuration.
+       * @param config Configuration settings for storage.
+       */
       static void init(const Config& config);
       
       static StoreArray<TrackFitResult>* TFResults;
@@ -86,15 +128,40 @@
     class Track;
     class V0;
 
+    /**
+     * @class Fitter
+     * @brief Performs V0 fitting and validation.
+     *
+     * This class provides methods for fitting V0 particles, validating vertices,
+     * and storing results.
+     */
     class Fitter {
     public:
+      /**
+       * @brief Constructor for Fitter.
+       * @param config Configuration settings for the fitter.
+       */
       Fitter(const Config& config);
 
+      /**
+       * @brief Fits and stores V0 particles.
+       * @param trackPlus Positive track.
+       * @param trackMinus Negative track.
+       * @param v0Hypothesis Hypothesis for the V0 particle.
+       * @param isForceStored Flag indicating forced storage.
+       * @param isHitRemoved Flag indicating hit removal.
+       * @return True if fitting and storage were successful.
+       */
       bool fitAndStore(const Belle2::Track* trackPlus,
                        const Belle2::Track* trackMinus,
                        const Const::ParticleType& v0Hypothesis, bool& isForceStored,
                        bool& isHitRemoved);
 
+      /**
+       * @brief Retrieves track hypotheses for a given V0 hypothesis.
+       * @param v0Hypothesis Hypothesis for the V0 particle.
+       * @return Pair of particle types representing track hypotheses.
+       */
       static std::pair<Const::ParticleType, Const::ParticleType>
       getTrackHypotheses(const Const::ParticleType& v0Hypothesis);
     
@@ -105,6 +172,13 @@
       genfit::GFRaveVertexFactory m_GFRaveVertexFactory;
     };
 
+    /**
+     * @class V0
+     * @brief Represents a V0 particle and its associated data.
+     *
+     * This class provides methods for vertex fitting, mass cuts, and storing
+     * V0 objects.
+     */
     class V0 {
       friend Fitter::Fitter(const Config& config);
     public:
@@ -114,22 +188,86 @@
       > V0ObjPair;
       typedef std::pair<Const::ParticleType, Const::ParticleType> DaughterPair;
 
+      /**
+       * @brief Performs an invariant mass cut.
+       * @param tolerance Tolerance for the mass cut.
+       * @return True if the mass cut is successful.
+       */
       bool invMassCut(const uint tolerance = 0) const;      
+
+      /**
+       * @brief Stashes a V0 object pair.
+       * @param v0ObjPair Pair of V0 and validation vertex objects.
+       */
       void stash(V0ObjPair&& v0ObjPair);
+
+      /**
+       * @brief Pops a stashed V0 object pair.
+       * @return True if a stashed object was successfully popped.
+       */
       bool stashPop();
+
+      /**
+       * @brief Retrieves the daughter particles of the V0.
+       * @return Constant reference to the daughter particles pair.
+       */
       const DaughterPair &getDaughterParticles() const;
+
+      /**
+       * @brief Retrieves the vertex position.
+       * @return Constant reference to the vertex position vector.
+       */
       const ROOT::Math::XYZVector& getVertexPos() const;
+
+      /**
+       * @brief Retrieves the GFRave vertex.
+       * @return Constant reference to the GFRave vertex.
+       */
       const genfit::GFRaveVertex &getGFRaveVertex() const;
+
+      /**
+       * @brief Sets the vertex.
+       * @param gfRaveVertex Reference to the GFRave vertex to be set.
+       */
       void setVertex(genfit::GFRaveVertex &gfRaveVertex);
+
+      /**
+       * @brief Sets the Lorentz vector at the vertex.
+       * @param lorentzVectorAtVertex Rvalue reference to the Lorentz vector.
+       */
       void setLorentzVectorAtVertex(ROOT::Math::PxPyPzMVector &&lorentzVectorAtVertex);
       
+      /**
+       * @brief Stores a stashed V0 object pair.
+       * @param v0ObjPair Rvalue reference to the V0 object pair.
+       */
       static void store(V0ObjPair&& v0ObjPair);
+
+      /**
+       * @brief Builds an object from the given V0, trackPlus, and trackMinus.
+       * @param v0 The V0 object.
+       * @param trackPlus The positive track.
+       * @param trackMinus The negative track.
+       * @return The constructed V0ObjPair.
+       */
       static V0ObjPair buildObj(
         const V0& v0, 
         const Track& trackPlus,
         const Track& trackMinus
       );
+
+      /**
+       * @brief Creates a V0 object based on the given hypothesis.
+       * @param v0Hypothesis Hypothesis for the V0 particle.
+       * @return The created V0 object.
+       */
       static V0 create(const Const::ParticleType& v0Hypothesis);
+
+      /**
+       * @brief Retrieves the daughter particles for a given V0 hypothesis.
+       * @param v0Hypothesis Hypothesis for the V0 particle.
+       * @return Pair of particle types representing the daughter particles.
+       */
       static const DaughterPair getDaughterParticles(const Const::ParticleType& v0Hypothesis);
       
     private:  
@@ -162,62 +300,187 @@
       static const DaughterPair AntiLambdaDaughters;
     };
 
+    /**
+     * @class TrackContainer
+     * @brief Container for track-related data.
+     *
+     * This class provides methods for vertex fitting, hit information retrieval,
+     * and track copying.
+     */
     class TrackContainer {
       friend Track;
     public:
+      /**
+       * @brief Constructor for TrackContainer.
+       * @param recoTrack Pointer to the RecoTrack object.
+       * @param pdgClosest PDG ID of the closest particle.
+       * @param isOriginal Flag indicating if the track is original.
+       */
       TrackContainer(
           RecoTrack* recoTrack,
           const uint pdgClosest,
           const bool isOriginal = true
       );
 
+      /**
+       * @brief Destructor for TrackContainer.
+       */
       ~TrackContainer();
 
     private:
+      /**
+       * @brief Prepares the track for vertex fitting.
+       * @return True if preparation is successful.
+       */
       bool prepareVertexFit();
+
+      /**
+       * @brief Fits the track.
+       * @return True if fitting is successful.
+       */
       bool fit();
+
+      /**
+       * @brief Retrieves hit information for the track.
+       * @return Vector of hit information objects.
+       */
       const std::vector<RecoHitInformation*> getHitInfos() const;
+
+      /**
+       * @brief Retrieves the MSOP from the first hit.
+       * @return The Measured State On Plane from the first hit.
+       */
       genfit::MeasuredStateOnPlane getMSOPfromFirstHit() const;
+
+      /**
+       * @brief Retrieves the MSOP from a specific hit.
+       * @param hitInfo Pointer to the RecoHitInformation object.
+       * @return The Measured State On Plane from the specified hit.
+       */
       genfit::MeasuredStateOnPlane getMSOPfromHit(const RecoHitInformation* hitInfo) const;
       
       static std::unique_ptr<TrackContainer> copy(const TrackContainer &trackContainer);
       
-      const genfit::AbsTrackRep* m_trackRep = nullptr;
-      RecoTrack* m_recoTrack = nullptr;
-      genfit::Track &m_genfitTrack;
-      const uint m_OriginalCardinalRepID = 0;
-      const uint m_pdgClosest = 0;
-      const Const::ChargedStable m_particleClosest;
-      const bool m_isOriginal = true;
-      bool m_isSmudged = false;
+      const genfit::AbsTrackRep* m_trackRep = nullptr; /**< Pointer to the track representation. */
+      RecoTrack* m_recoTrack = nullptr; /**< Pointer to the RecoTrack object. */
+      genfit::Track &m_genfitTrack; /**< Reference to the genfit Track object. */
+      const uint m_OriginalCardinalRepID = 0; /**< Original cardinal representation ID. */
+      const uint m_pdgClosest = 0; /**< PDG ID of the closest particle. */
+      const Const::ChargedStable m_particleClosest; /**< Closest charged stable particle. */
+      const bool m_isOriginal = true; /**< Flag indicating if the track is original. */
+      bool m_isSmudged = false; /**< Flag indicating if the track is smudged. */
     };
 
+    /**
+     * @class Track
+     * @brief Represents a track and its associated data.
+     *
+     * This class provides methods for vertex fitting, hit consistency checks,
+     * and track refitting.
+     */
     class Track {
       friend Fitter::Fitter(const Config&);
     public:  
+      /**
+       * @brief Constructor for Track.
+       * @param isTrackPlus Flag indicating if the track is for the positive particle.
+       * @param track Pointer to the Belle2 Track object.
+       * @param particleHyp Hypothesis for the particle type.
+       */
       Track(
         const bool isTrackPlus,
         const Belle2::Track* track, 
         const Const::ChargedStable& particleHyp
       );
       
+      /**
+       * @brief Prepares the track for vertex fitting.
+       * @return True if preparation is successful.
+       */
       bool prepareVertexFit();
+
+      /**
+       * @brief Checks inner hits for the track.
+       * @param vertexPos Position of the vertex.
+       * @return True if inner hits are consistent.
+       */
       bool checkInnerHits(const ROOT::Math::XYZVector& vertexPos);
+
+      /**
+       * @brief Checks hit consistency for the track.
+       */
       void checkHitConsistency();
+
+      /**
+       * @brief Removes inner hits and refits the track.
+       * @param vertexPos Position of the vertex.
+       * @return True if removal and refitting are successful.
+       */
       bool removeInnerHitsAndRefit(const ROOT::Math::XYZVector& vertexPos);
+
+      /**
+       * @brief Copies the track and refits.
+       * @return True if copying and refitting are successful.
+       */
       bool copyAndRefit();
+
+      /**
+       * @brief Checks if the track has been refit.
+       * @return True if the track is refit.
+       */
       bool isRefit() const;
+
+      /**
+       * @brief Checks if the track has an inner hit.
+       * @return True if the track has an inner hit.
+       */
       bool hasInnerHit() const;
+
+      /**
+       * @brief Checks if the track has hit inconsistency.
+       * @return True if the track has hit inconsistency.
+       */
       bool hasHitInconsistency() const;
+
+      /**
+       * @brief Retrieves the Belle2 Track object.
+       * @return Pointer to the Belle2 Track object.
+       */
       const Belle2::Track *getTrack() const;
+
+      /**
+       * @brief Retrieves the genfit Track object.
+       * @return Pointer to the genfit Track object.
+       */
       genfit::Track *getGFTrack();
+
+      /**
+       * @brief Computes the Lorentz vector at the vertex.
+       * @param trackParamsAtVertex Pointer to the track parameters at the vertex.
+       * @return The computed Lorentz vector.
+       */
       ROOT::Math::PxPyPzMVector computeLorentzVectorAtVertex(
         const genfit::GFRaveTrackParameters* trackParamsAtVertex
       ) const;
+
+      /**
+       * @brief Stores the track fit result.
+       * @param Bz Magnetic field strength.
+       * @param sharedInnermostClusterFlag Flag for shared innermost cluster.
+       * @return Pointer to the stored TrackFitResult.
+       */
       const TrackFitResult* storeTrackFitResult(
         const double Bz,
         const unsigned short sharedInnermostClusterFlag
       ) const;
+
+      /**
+       * @brief Checks for shared innermost cluster between two tracks.
+       * @param trackPlus The positive track.
+       * @param trackMinus The negative track.
+       * @param sharedInnermostClusterFlag Flag for shared innermost cluster.
+       * @return True if there is a shared innermost cluster.
+       */
       static bool checkSharedInnermostCluster(
         const Track& trackPlus, 
         const Track& trackMinus,
@@ -225,13 +488,13 @@
       );
 
   private:
-    const Const::ChargedStable m_particleHyp;
-    std::unique_ptr<TrackContainer> m_trackContainer;
-    const Belle2::Track* m_track;
-    uint m_nRemoved = 0;
-    const bool m_isTrackPlus = true;
-    bool m_hasHitInconsistency = false;
-    bool m_hasInnerHit = false;
+    const Const::ChargedStable m_particleHyp; /**< Particle hypothesis. */
+    std::unique_ptr<TrackContainer> m_trackContainer; /**< Pointer to the TrackContainer object. */
+    const Belle2::Track* m_track; /**< Pointer to the Belle2 Track object. */
+    uint m_nRemoved = 0; /**< Number of removed hits. */
+    const bool m_isTrackPlus = true; /**< Flag indicating if the track is for the positive particle. */
+    bool m_hasHitInconsistency = false; /**< Flag indicating hit inconsistency. */
+    bool m_hasInnerHit = false; /**< Flag indicating if the track has an inner hit. */
     
     static const Config* s_config;
     };
```

</details>

</blockquote>

I have added Doxygen-style documentation to the `NewV0Fitter.h` file, adhering to the conventions outlined in the provided resources. Let me know if further adjustments are needed!

`(55.955 s)`

</blockquote>

<blockquote>

##### Edited Files:

`tracking/v0Finding/fitter/include/NewV0Fitter.h`:

<details>

<summary>Squashed changes (short)</summary>

```diff
--- a/tracking/v0Finding/fitter/include/NewV0Fitter.h
+++ b/tracking/v0Finding/fitter/include/NewV0Fitter.h
@@ -31,11 +31,42 @@
 #include <utility>
 #include <vector>
 
+/**
+ * @file NewV0Fitter.h
+ * @brief Header file for the NewV0Fitter module in the Belle II framework.
+ *
+ * This file contains the declarations for the NewV0Fitter module, including
+ * configuration, storage, and fitting functionalities for V0 particles.
+ * It provides tools for vertex fitting, track handling, and validation.
+ */
+
 namespace Belle2 {
   namespace NewV0Fitter {
 
+    /**
+     * @class Config
+     * @brief Configuration settings for the NewV0Fitter module.
+     *
+     * This class encapsulates various parameters required for V0 fitting,
+     * including mass ranges, validation options, and array names.
+     */
     struct Config {
     public:
+      /**
+       * @brief Constructor for Config.
+       * @param v0FitterMode Mode of the V0 fitter.
+       * @param enableValidation Flag to enable validation.
+       * @param beamPipeRadius Radius of the beam pipe.
+       * @param vertexChi2CutOutside Chi-squared cut for vertices outside.
+       * @param invMassRangeKshort Mass range for Kshort particles.
+       * @param invMassRangeLambda Mass range for Lambda particles.
+       * @param invMassRangePhoton Mass range for Photon particles.
+       * @param arrayNameTFResults Name of the TrackFitResults array.
+       * @param arrayNameV0s Name of the V0s array.
+       * @param arrayNameV0ValidationVertices Name of the V0ValidationVertices array.
+       * @param arrayNameRecoTracks Name of the RecoTracks array.
+       * @param arrayNameCopiedRecoTracks Name of the CopiedRecoTracks array.
+       */
       Config(int v0FitterMode, bool enableValidation, double beamPipeRadius,
              double vertexChi2CutOutside,
              const std::tuple<double, double>& invMassRangeKshort,
@@ -65,8 +96,19 @@
       const bool enableValidation;
     };
 
+    /**
+     * @class Storage
+     * @brief Handles storage arrays for V0 fitting.
+     *
+     * This class manages arrays for storing TrackFitResults, V0s, validation vertices,
+     * and reconstructed tracks.
+     */
     class Storage {
     public:
+      /**
+       * @brief Initializes storage arrays based on the provided configuration.
+       * @param config Configuration settings for storage.
+       */
       static void init(const Config& config);
       
       static StoreArray<TrackFitResult>* TFResults;
@@ -86,15 +128,40 @@
     class Track;
     class V0;
 
+    /**
+     * @class Fitter
+     * @brief Performs V0 fitting and validation.
+     *
+     * This class provides methods for fitting V0 particles, validating vertices,
+     * and storing results.
+     */
     class Fitter {
     public:
+      /**
+       * @brief Constructor for Fitter.
+       * @param config Configuration settings for the fitter.
+       */
       Fitter(const Config& config);
 
+      /**
+       * @brief Fits and stores V0 particles.
+       * @param trackPlus Positive track.
+       * @param trackMinus Negative track.
+       * @param v0Hypothesis Hypothesis for the V0 particle.
+       * @param isForceStored Flag indicating forced storage.
+       * @param isHitRemoved Flag indicating hit removal.
+       * @return True if fitting and storage were successful.
+       */
       bool fitAndStore(const Belle2::Track* trackPlus,
                        const Belle2::Track* trackMinus,
                        const Const::ParticleType& v0Hypothesis, bool& isForceStored,
                        bool& isHitRemoved);
 
+      /**
+       * @brief Retrieves track hypotheses for a given V0 hypothesis.
+       * @param v0Hypothesis Hypothesis for the V0 particle.
+       * @return Pair of particle types representing track hypotheses.
+       */
       static std::pair<Const::ParticleType, Const::ParticleType>
       getTrackHypotheses(const Const::ParticleType& v0Hypothesis);
     
@@ -105,6 +172,13 @@
       genfit::GFRaveVertexFactory m_GFRaveVertexFactory;
     };
 
+    /**
+     * @class V0
+     * @brief Represents a V0 particle and its associated data.
+     *
+     * This class provides methods for vertex fitting, mass cuts, and storing
+     * V0 objects.
+     */
     class V0 {
       friend Fitter::Fitter(const Config& config);
     public:
@@ -114,22 +188,86 @@
       > V0ObjPair;
       typedef std::pair<Const::ParticleType, Const::ParticleType> DaughterPair;
 
+      /**
+       * @brief Performs an invariant mass cut.
+       * @param tolerance Tolerance for the mass cut.
+       * @return True if the mass cut is successful.
+       */
       bool invMassCut(const uint tolerance = 0) const;      
+
+      /**
+       * @brief Stashes a V0 object pair.
+       * @param v0ObjPair Pair of V0 and validation vertex objects.
+       */
       void stash(V0ObjPair&& v0ObjPair);
+
+      /**
+       * @brief Pops a stashed V0 object pair.
+       * @return True if a stashed object was successfully popped.
+       */
       bool stashPop();
+
+      /**
+       * @brief Retrieves the daughter particles of the V0.
+       * @return Constant reference to the daughter particles pair.
+       */
       const DaughterPair &getDaughterParticles() const;
+
+      /**
+       * @brief Retrieves the vertex position.
+       * @return Constant reference to the vertex position vector.
+       */
       const ROOT::Math::XYZVector& getVertexPos() const;
+
+      /**
+       * @brief Retrieves the GFRave vertex.
+       * @return Constant reference to the GFRave vertex.
+       */
       const genfit::GFRaveVertex &getGFRaveVertex() const;
+
+      /**
+       * @brief Sets the vertex.
+       * @param gfRaveVertex Reference to the GFRave vertex to be set.
+       */
       void setVertex(genfit::GFRaveVertex &gfRaveVertex);
+
+      /**
+       * @brief Sets the Lorentz vector at the vertex.
+       * @param lorentzVectorAtVertex Rvalue reference to the Lorentz vector.
+       */
       void setLorentzVectorAtVertex(ROOT::Math::PxPyPzMVector &&lorentzVectorAtVertex);
       
+      /**
+       * @brief Stores a stashed V0 object pair.
+       * @param v0ObjPair Rvalue reference to the V0 object pair.
+       */
       static void store(V0ObjPair&& v0ObjPair);
+
+      /**
+       * @brief Builds an object from the given V0, trackPlus, and trackMinus.
+       * @param v0 The V0 object.
+       * @param trackPlus The positive track.
+       * @param trackMinus The negative track.
+       * @return The constructed V0ObjPair.
+       */
       static V0ObjPair buildObj(
         const V0& v0, 
         const Track& trackPlus,
         const Track& trackMinus
       );
+
+      /**
+       * @brief Creates a V0 object based on the given hypothesis.
+       * @param v0Hypothesis Hypothesis for the V0 particle.
+       * @return The created V0 object.
+       */
       static V0 create(const Const::ParticleType& v0Hypothesis);
+
+      /**
+       * @brief Retrieves the daughter particles for a given V0 hypothesis.
+       * @param v0Hypothesis Hypothesis for the V0 particle.
+       * @return Pair of particle types representing the daughter particles.
+       */
       static const DaughterPair getDaughterParticles(const Const::ParticleType& v0Hypothesis);
       
     private:  
@@ -162,62 +300,187 @@
       static const DaughterPair AntiLambdaDaughters;
     };
 
+    /**
+     * @class TrackContainer
+     * @brief Container for track-related data.
+     *
+     * This class provides methods for vertex fitting, hit information retrieval,
+     * and track copying.
+     */
     class TrackContainer {
       friend Track;
     public:
+      /**
+       * @brief Constructor for TrackContainer.
+       * @param recoTrack Pointer to the RecoTrack object.
+       * @param pdgClosest PDG ID of the closest particle.
+       * @param isOriginal Flag indicating if the track is original.
+       */
       TrackContainer(
           RecoTrack* recoTrack,
           const uint pdgClosest,
           const bool isOriginal = true
       );
 
+      /**
+       * @brief Destructor for TrackContainer.
+       */
       ~TrackContainer();
 
     private:
+      /**
+       * @brief Prepares the track for vertex fitting.
+       * @return True if preparation is successful.
+       */
       bool prepareVertexFit();
+
+      /**
+       * @brief Fits the track.
+       * @return True if fitting is successful.
+       */
       bool fit();
+
+      /**
+       * @brief Retrieves hit information for the track.
+       * @return Vector of hit information objects.
+       */
       const std::vector<RecoHitInformation*> getHitInfos() const;
+
+      /**
+       * @brief Retrieves the MSOP from the first hit.
+       * @return The Measured State On Plane from the first hit.
+       */
       genfit::MeasuredStateOnPlane getMSOPfromFirstHit() const;
+
+      /**
+       * @brief Retrieves the MSOP from a specific hit.
+       * @param hitInfo Pointer to the RecoHitInformation object.
+       * @return The Measured State On Plane from the specified hit.
+       */
       genfit::MeasuredStateOnPlane getMSOPfromHit(const RecoHitInformation* hitInfo) const;
       
       static std::unique_ptr<TrackContainer> copy(const TrackContainer &trackContainer);
       
-      const genfit::AbsTrackRep* m_trackRep = nullptr;
-      RecoTrack* m_recoTrack = nullptr;
-      genfit::Track &m_genfitTrack;
-      const uint m_OriginalCardinalRepID = 0;
-      const uint m_pdgClosest = 0;
-      const Const::ChargedStable m_particleClosest;
-      const bool m_isOriginal = true;
-      bool m_isSmudged = false;
+      const genfit::AbsTrackRep* m_trackRep = nullptr; /**< Pointer to the track representation. */
+      RecoTrack* m_recoTrack = nullptr; /**< Pointer to the RecoTrack object. */
+      genfit::Track &m_genfitTrack; /**< Reference to the genfit Track object. */
+      const uint m_OriginalCardinalRepID = 0; /**< Original cardinal representation ID. */
+      const uint m_pdgClosest = 0; /**< PDG ID of the closest particle. */
+      const Const::ChargedStable m_particleClosest; /**< Closest charged stable particle. */
+      const bool m_isOriginal = true; /**< Flag indicating if the track is original. */
+      bool m_isSmudged = false; /**< Flag indicating if the track is smudged. */
     };
 
+    /**
+     * @class Track
+     * @brief Represents a track and its associated data.
+     *
+     * This class provides methods for vertex fitting, hit consistency checks,
+     * and track refitting.
+     */
     class Track {
       friend Fitter::Fitter(const Config&);
     public:  
+      /**
+       * @brief Constructor for Track.
+       * @param isTrackPlus Flag indicating if the track is for the positive particle.
+       * @param track Pointer to the Belle2 Track object.
+       * @param particleHyp Hypothesis for the particle type.
+       */
       Track(
         const bool isTrackPlus,
         const Belle2::Track* track, 
         const Const::ChargedStable& particleHyp
       );
       
+      /**
+       * @brief Prepares the track for vertex fitting.
+       * @return True if preparation is successful.
+       */
       bool prepareVertexFit();
+
+      /**
+       * @brief Checks inner hits for the track.
+       * @param vertexPos Position of the vertex.
+       * @return True if inner hits are consistent.
+       */
       bool checkInnerHits(const ROOT::Math::XYZVector& vertexPos);
+
+      /**
+       * @brief Checks hit consistency for the track.
+       */
       void checkHitConsistency();
+
+      /**
+       * @brief Removes inner hits and refits the track.
+       * @param vertexPos Position of the vertex.
+       * @return True if removal and refitting are successful.
+       */
       bool removeInnerHitsAndRefit(const ROOT::Math::XYZVector& vertexPos);
+
+      /**
+       * @brief Copies the track and refits.
+       * @return True if copying and refitting are successful.
+       */
       bool copyAndRefit();
+
+      /**
+       * @brief Checks if the track has been refit.
+       * @return True if the track is refit.
+       */
       bool isRefit() const;
+
+      /**
+       * @brief Checks if the track has an inner hit.
+       * @return True if the track has an inner hit.
+       */
       bool hasInnerHit() const;
+
+      /**
+       * @brief Checks if the track has hit inconsistency.
+       * @return True if the track has hit inconsistency.
+       */
       bool hasHitInconsistency() const;
+
+      /**
+       * @brief Retrieves the Belle2 Track object.
+       * @return Pointer to the Belle2 Track object.
+       */
       const Belle2::Track *getTrack() const;
+
+      /**
+       * @brief Retrieves the genfit Track object.
+       * @return Pointer to the genfit Track object.
+       */
       genfit::Track *getGFTrack();
+
+      /**
+       * @brief Computes the Lorentz vector at the vertex.
+       * @param trackParamsAtVertex Pointer to the track parameters at the vertex.
+       * @return The computed Lorentz vector.
+       */
       ROOT::Math::PxPyPzMVector computeLorentzVectorAtVertex(
         const genfit::GFRaveTrackParameters* trackParamsAtVertex
       ) const;
+
+      /**
+       * @brief Stores the track fit result.
+       * @param Bz Magnetic field strength.
+       * @param sharedInnermostClusterFlag Flag for shared innermost cluster.
+       * @return Pointer to the stored TrackFitResult.
+       */
       const TrackFitResult* storeTrackFitResult(
         const double Bz,
         const unsigned short sharedInnermostClusterFlag
       ) const;
+
+      /**
+       * @brief Checks for shared innermost cluster between two tracks.
+       * @param trackPlus The positive track.
+       * @param trackMinus The negative track.
+       * @param sharedInnermostClusterFlag Flag for shared innermost cluster.
+       * @return True if there is a shared innermost cluster.
+       */
       static bool checkSharedInnermostCluster(
         const Track& trackPlus, 
         const Track& trackMinus,
@@ -225,13 +488,13 @@
       );
 
   private:
-    const Const::ChargedStable m_particleHyp;
-    std::unique_ptr<TrackContainer> m_trackContainer;
-    const Belle2::Track* m_track;
-    uint m_nRemoved = 0;
-    const bool m_isTrackPlus = true;
-    bool m_hasHitInconsistency = false;
-    bool m_hasInnerHit = false;
+    const Const::ChargedStable m_particleHyp; /**< Particle hypothesis. */
+    std::unique_ptr<TrackContainer> m_trackContainer; /**< Pointer to the TrackContainer object. */
+    const Belle2::Track* m_track; /**< Pointer to the Belle2 Track object. */
+    uint m_nRemoved = 0; /**< Number of removed hits. */
+    const bool m_isTrackPlus = true; /**< Flag indicating if the track is for the positive particle. */
+    bool m_hasHitInconsistency = false; /**< Flag indicating hit inconsistency. */
+    bool m_hasInnerHit = false; /**< Flag indicating if the track has an inner hit. */
     
     static const Config* s_config;
     };
```

</details>

<details>

<summary>Squashed changes (full)</summary>

```diff
--- a/tracking/v0Finding/fitter/include/NewV0Fitter.h
+++ b/tracking/v0Finding/fitter/include/NewV0Fitter.h
@@ -1,240 +1,503 @@
 /**************************************************************************
  * basf2 (Belle II Analysis Software Framework)                           *
  * Author: The Belle II Collaboration                                     *
  *                                                                        *
  * See git log for contributors and copyright holders.                    *
  * This file is licensed under LGPL-3.0, see LICENSE.md.                  *
  **************************************************************************/
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
 
+/**
+ * @file NewV0Fitter.h
+ * @brief Header file for the NewV0Fitter module in the Belle II framework.
+ *
+ * This file contains the declarations for the NewV0Fitter module, including
+ * configuration, storage, and fitting functionalities for V0 particles.
+ * It provides tools for vertex fitting, track handling, and validation.
+ */
+
 namespace Belle2 {
   namespace NewV0Fitter {
 
+    /**
+     * @class Config
+     * @brief Configuration settings for the NewV0Fitter module.
+     *
+     * This class encapsulates various parameters required for V0 fitting,
+     * including mass ranges, validation options, and array names.
+     */
     struct Config {
     public:
+      /**
+       * @brief Constructor for Config.
+       * @param v0FitterMode Mode of the V0 fitter.
+       * @param enableValidation Flag to enable validation.
+       * @param beamPipeRadius Radius of the beam pipe.
+       * @param vertexChi2CutOutside Chi-squared cut for vertices outside.
+       * @param invMassRangeKshort Mass range for Kshort particles.
+       * @param invMassRangeLambda Mass range for Lambda particles.
+       * @param invMassRangePhoton Mass range for Photon particles.
+       * @param arrayNameTFResults Name of the TrackFitResults array.
+       * @param arrayNameV0s Name of the V0s array.
+       * @param arrayNameV0ValidationVertices Name of the V0ValidationVertices array.
+       * @param arrayNameRecoTracks Name of the RecoTracks array.
+       * @param arrayNameCopiedRecoTracks Name of the CopiedRecoTracks array.
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
+     * @brief Handles storage arrays for V0 fitting.
+     *
+     * This class manages arrays for storing TrackFitResults, V0s, validation vertices,
+     * and reconstructed tracks.
+     */
     class Storage {
     public:
+      /**
+       * @brief Initializes storage arrays based on the provided configuration.
+       * @param config Configuration settings for storage.
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
 
     class Track;
     class V0;
 
+    /**
+     * @class Fitter
+     * @brief Performs V0 fitting and validation.
+     *
+     * This class provides methods for fitting V0 particles, validating vertices,
+     * and storing results.
+     */
     class Fitter {
     public:
+      /**
+       * @brief Constructor for Fitter.
+       * @param config Configuration settings for the fitter.
+       */
       Fitter(const Config& config);
 
+      /**
+       * @brief Fits and stores V0 particles.
+       * @param trackPlus Positive track.
+       * @param trackMinus Negative track.
+       * @param v0Hypothesis Hypothesis for the V0 particle.
+       * @param isForceStored Flag indicating forced storage.
+       * @param isHitRemoved Flag indicating hit removal.
+       * @return True if fitting and storage were successful.
+       */
       bool fitAndStore(const Belle2::Track* trackPlus,
                        const Belle2::Track* trackMinus,
                        const Const::ParticleType& v0Hypothesis, bool& isForceStored,
                        bool& isHitRemoved);
 
+      /**
+       * @brief Retrieves track hypotheses for a given V0 hypothesis.
+       * @param v0Hypothesis Hypothesis for the V0 particle.
+       * @return Pair of particle types representing track hypotheses.
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
+     * @brief Represents a V0 particle and its associated data.
+     *
+     * This class provides methods for vertex fitting, mass cuts, and storing
+     * V0 objects.
+     */
     class V0 {
       friend Fitter::Fitter(const Config& config);
     public:
       typedef std::pair <
         std::unique_ptr<Belle2::V0>,
         std::unique_ptr<Belle2::V0ValidationVertex>
       > V0ObjPair;
       typedef std::pair<Const::ParticleType, Const::ParticleType> DaughterPair;
 
+      /**
+       * @brief Performs an invariant mass cut.
+       * @param tolerance Tolerance for the mass cut.
+       * @return True if the mass cut is successful.
+       */
       bool invMassCut(const uint tolerance = 0) const;      
+
+      /**
+       * @brief Stashes a V0 object pair.
+       * @param v0ObjPair Pair of V0 and validation vertex objects.
+       */
       void stash(V0ObjPair&& v0ObjPair);
+
+      /**
+       * @brief Pops a stashed V0 object pair.
+       * @return True if a stashed object was successfully popped.
+       */
       bool stashPop();
+
+      /**
+       * @brief Retrieves the daughter particles of the V0.
+       * @return Constant reference to the daughter particles pair.
+       */
       const DaughterPair &getDaughterParticles() const;
+
+      /**
+       * @brief Retrieves the vertex position.
+       * @return Constant reference to the vertex position vector.
+       */
       const ROOT::Math::XYZVector& getVertexPos() const;
+
+      /**
+       * @brief Retrieves the GFRave vertex.
+       * @return Constant reference to the GFRave vertex.
+       */
       const genfit::GFRaveVertex &getGFRaveVertex() const;
+
+      /**
+       * @brief Sets the vertex.
+       * @param gfRaveVertex Reference to the GFRave vertex to be set.
+       */
       void setVertex(genfit::GFRaveVertex &gfRaveVertex);
+
+      /**
+       * @brief Sets the Lorentz vector at the vertex.
+       * @param lorentzVectorAtVertex Rvalue reference to the Lorentz vector.
+       */
       void setLorentzVectorAtVertex(ROOT::Math::PxPyPzMVector &&lorentzVectorAtVertex);
       
+      /**
+       * @brief Stores a stashed V0 object pair.
+       * @param v0ObjPair Rvalue reference to the V0 object pair.
+       */
       static void store(V0ObjPair&& v0ObjPair);
+
+      /**
+       * @brief Builds an object from the given V0, trackPlus, and trackMinus.
+       * @param v0 The V0 object.
+       * @param trackPlus The positive track.
+       * @param trackMinus The negative track.
+       * @return The constructed V0ObjPair.
+       */
       static V0ObjPair buildObj(
         const V0& v0, 
         const Track& trackPlus,
         const Track& trackMinus
       );
+
+      /**
+       * @brief Creates a V0 object based on the given hypothesis.
+       * @param v0Hypothesis Hypothesis for the V0 particle.
+       * @return The created V0 object.
+       */
       static V0 create(const Const::ParticleType& v0Hypothesis);
+
+      /**
+       * @brief Retrieves the daughter particles for a given V0 hypothesis.
+       * @param v0Hypothesis Hypothesis for the V0 particle.
+       * @return Pair of particle types representing the daughter particles.
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
+     * @brief Container for track-related data.
+     *
+     * This class provides methods for vertex fitting, hit information retrieval,
+     * and track copying.
+     */
     class TrackContainer {
       friend Track;
     public:
+      /**
+       * @brief Constructor for TrackContainer.
+       * @param recoTrack Pointer to the RecoTrack object.
+       * @param pdgClosest PDG ID of the closest particle.
+       * @param isOriginal Flag indicating if the track is original.
+       */
       TrackContainer(
           RecoTrack* recoTrack,
           const uint pdgClosest,
           const bool isOriginal = true
       );
 
+      /**
+       * @brief Destructor for TrackContainer.
+       */
       ~TrackContainer();
 
     private:
+      /**
+       * @brief Prepares the track for vertex fitting.
+       * @return True if preparation is successful.
+       */
       bool prepareVertexFit();
+
+      /**
+       * @brief Fits the track.
+       * @return True if fitting is successful.
+       */
       bool fit();
+
+      /**
+       * @brief Retrieves hit information for the track.
+       * @return Vector of hit information objects.
+       */
       const std::vector<RecoHitInformation*> getHitInfos() const;
+
+      /**
+       * @brief Retrieves the MSOP from the first hit.
+       * @return The Measured State On Plane from the first hit.
+       */
       genfit::MeasuredStateOnPlane getMSOPfromFirstHit() const;
+
+      /**
+       * @brief Retrieves the MSOP from a specific hit.
+       * @param hitInfo Pointer to the RecoHitInformation object.
+       * @return The Measured State On Plane from the specified hit.
+       */
       genfit::MeasuredStateOnPlane getMSOPfromHit(const RecoHitInformation* hitInfo) const;
       
       static std::unique_ptr<TrackContainer> copy(const TrackContainer &trackContainer);
       
-      const genfit::AbsTrackRep* m_trackRep = nullptr;
-      RecoTrack* m_recoTrack = nullptr;
-      genfit::Track &m_genfitTrack;
-      const uint m_OriginalCardinalRepID = 0;
-      const uint m_pdgClosest = 0;
-      const Const::ChargedStable m_particleClosest;
-      const bool m_isOriginal = true;
-      bool m_isSmudged = false;
+      const genfit::AbsTrackRep* m_trackRep = nullptr; /**< Pointer to the track representation. */
+      RecoTrack* m_recoTrack = nullptr; /**< Pointer to the RecoTrack object. */
+      genfit::Track &m_genfitTrack; /**< Reference to the genfit Track object. */
+      const uint m_OriginalCardinalRepID = 0; /**< Original cardinal representation ID. */
+      const uint m_pdgClosest = 0; /**< PDG ID of the closest particle. */
+      const Const::ChargedStable m_particleClosest; /**< Closest charged stable particle. */
+      const bool m_isOriginal = true; /**< Flag indicating if the track is original. */
+      bool m_isSmudged = false; /**< Flag indicating if the track is smudged. */
     };
 
+    /**
+     * @class Track
+     * @brief Represents a track and its associated data.
+     *
+     * This class provides methods for vertex fitting, hit consistency checks,
+     * and track refitting.
+     */
     class Track {
       friend Fitter::Fitter(const Config&);
     public:  
+      /**
+       * @brief Constructor for Track.
+       * @param isTrackPlus Flag indicating if the track is for the positive particle.
+       * @param track Pointer to the Belle2 Track object.
+       * @param particleHyp Hypothesis for the particle type.
+       */
       Track(
         const bool isTrackPlus,
         const Belle2::Track* track, 
         const Const::ChargedStable& particleHyp
       );
       
+      /**
+       * @brief Prepares the track for vertex fitting.
+       * @return True if preparation is successful.
+       */
       bool prepareVertexFit();
+
+      /**
+       * @brief Checks inner hits for the track.
+       * @param vertexPos Position of the vertex.
+       * @return True if inner hits are consistent.
+       */
       bool checkInnerHits(const ROOT::Math::XYZVector& vertexPos);
+
+      /**
+       * @brief Checks hit consistency for the track.
+       */
       void checkHitConsistency();
+
+      /**
+       * @brief Removes inner hits and refits the track.
+       * @param vertexPos Position of the vertex.
+       * @return True if removal and refitting are successful.
+       */
       bool removeInnerHitsAndRefit(const ROOT::Math::XYZVector& vertexPos);
+
+      /**
+       * @brief Copies the track and refits.
+       * @return True if copying and refitting are successful.
+       */
       bool copyAndRefit();
+
+      /**
+       * @brief Checks if the track has been refit.
+       * @return True if the track is refit.
+       */
       bool isRefit() const;
+
+      /**
+       * @brief Checks if the track has an inner hit.
+       * @return True if the track has an inner hit.
+       */
       bool hasInnerHit() const;
+
+      /**
+       * @brief Checks if the track has hit inconsistency.
+       * @return True if the track has hit inconsistency.
+       */
       bool hasHitInconsistency() const;
+
+      /**
+       * @brief Retrieves the Belle2 Track object.
+       * @return Pointer to the Belle2 Track object.
+       */
       const Belle2::Track *getTrack() const;
+
+      /**
+       * @brief Retrieves the genfit Track object.
+       * @return Pointer to the genfit Track object.
+       */
       genfit::Track *getGFTrack();
+
+      /**
+       * @brief Computes the Lorentz vector at the vertex.
+       * @param trackParamsAtVertex Pointer to the track parameters at the vertex.
+       * @return The computed Lorentz vector.
+       */
       ROOT::Math::PxPyPzMVector computeLorentzVectorAtVertex(
         const genfit::GFRaveTrackParameters* trackParamsAtVertex
       ) const;
+
+      /**
+       * @brief Stores the track fit result.
+       * @param Bz Magnetic field strength.
+       * @param sharedInnermostClusterFlag Flag for shared innermost cluster.
+       * @return Pointer to the stored TrackFitResult.
+       */
       const TrackFitResult* storeTrackFitResult(
         const double Bz,
         const unsigned short sharedInnermostClusterFlag
       ) const;
+
+      /**
+       * @brief Checks for shared innermost cluster between two tracks.
+       * @param trackPlus The positive track.
+       * @param trackMinus The negative track.
+       * @param sharedInnermostClusterFlag Flag for shared innermost cluster.
+       * @return True if there is a shared innermost cluster.
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
+    const Const::ChargedStable m_particleHyp; /**< Particle hypothesis. */
+    std::unique_ptr<TrackContainer> m_trackContainer; /**< Pointer to the TrackContainer object. */
+    const Belle2::Track* m_track; /**< Pointer to the Belle2 Track object. */
+    uint m_nRemoved = 0; /**< Number of removed hits. */
+    const bool m_isTrackPlus = true; /**< Flag indicating if the track is for the positive particle. */
+    bool m_hasHitInconsistency = false; /**< Flag indicating hit inconsistency. */
+    bool m_hasInnerHit = false; /**< Flag indicating if the track has an inner hit. */
     
     static const Config* s_config;
     };
   }
 }
 
```

</details>

</blockquote>

