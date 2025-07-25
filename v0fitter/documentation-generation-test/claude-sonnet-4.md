Document ID: `chat-logs/1521322`

<blockquote>

#### philipobi:
**Task**: Add Doxygen-style documentation to #file:NewV0Fitter.h   
**Requirements**:
- Be sure to add documentation for **every** class/struct, method and data member (public & private).
- Use `@class` and `@brief` for class/struct documentation
- Use `@brief`, `@param` (and `@return` for non-void returntypes) for method documentation
- Use `///<` inline for member documentation
- Keep the documentation concise, meaningful, and strictly comment-based — **do not remove anything, only add comments**. 
- Refer to #file:NewV0Fitter.cc   for implementation details, but do not modify it.
- The NewV0Fitter is a refactored version of the V0Fitter, so use the documentation in #file:V0Fitter.h  as a point of reference but **do not copy any code** directly from it.


Variables: `file:V0Fitter.h`, `file:NewV0Fitter.cc`, `file:NewV0Fitter.h`

</blockquote>

<blockquote>

#### GitHub Copilot (claude-sonnet-4):

I'll analyze the code and add comprehensive Doxygen-style documentation to the `NewV0Fitter.h` file. Let me first examine the current structure and then add the required documentation. 

<blockquote>

Read `tracking/v0Finding/fitter/include/NewV0Fitter.h`, lines 1 to 50

</blockquote>

Now let me read the rest of the file to understand the complete structure: 

<blockquote>

Read `tracking/v0Finding/fitter/include/NewV0Fitter.h`, lines 50 to 240

</blockquote>

Now I'll add comprehensive Doxygen documentation to the header file. I'll start by adding documentation for the Config struct: 

<blockquote>

Edited `tracking/v0Finding/fitter/include/NewV0Fitter.h`

<details>

```diff
--- a/tracking/v0Finding/fitter/include/NewV0Fitter.h
+++ b/tracking/v0Finding/fitter/include/NewV0Fitter.h
@@ -34,8 +34,30 @@
 namespace Belle2 {
   namespace NewV0Fitter {
 
+    /**
+     * @class Config
+     * @brief Configuration structure for the NewV0Fitter containing all necessary parameters and settings.
+     * 
+     * This structure holds all configuration parameters needed for V0 fitting, including fitter modes,
+     * validation settings, mass ranges, and DataStore array names.
+     */
     struct Config {
     public:
+      /**
+       * @brief Constructor for the Config structure.
+       * @param v0FitterMode V0 fitter mode (0: store at first fit, 1: remove inner hits, 2: mode 1 + SVD restrictions)
+       * @param enableValidation Whether to enable validation vertex creation
+       * @param beamPipeRadius Radius defining inside/outside beam pipe boundary
+       * @param vertexChi2CutOutside Chi2 cut for vertices outside beam pipe
+       * @param invMassRangeKshort Invariant mass range for K-short hypothesis
+       * @param invMassRangeLambda Invariant mass range for Lambda hypothesis
+       * @param invMassRangePhoton Invariant mass range for photon hypothesis
+       * @param arrayNameTFResults Name of TrackFitResult array in DataStore
+       * @param arrayNameV0s Name of V0 array in DataStore
+       * @param arrayNameV0ValidationVertices Name of V0ValidationVertex array in DataStore
+       * @param arrayNameRecoTracks Name of RecoTrack array in DataStore
+       * @param arrayNameCopiedRecoTracks Name of copied RecoTrack array in DataStore
+       */
       Config(int v0FitterMode, bool enableValidation, double beamPipeRadius,
              double vertexChi2CutOutside,
              const std::tuple<double, double>& invMassRangeKshort,

```

</details>

</blockquote>

Now I'll add documentation for the Config struct members: 

<blockquote>

Edited `tracking/v0Finding/fitter/include/NewV0Fitter.h`

<details>

```diff
--- a/tracking/v0Finding/fitter/include/NewV0Fitter.h
+++ b/tracking/v0Finding/fitter/include/NewV0Fitter.h
@@ -69,22 +69,22 @@
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
+      const std::string arrayNameV0ValidationVertices; ///< Name of V0ValidationVertex array in DataStore
+      const std::string arrayNameRecoTracks; ///< Name of RecoTrack array in DataStore
+      const std::string arrayNameCopiedRecoTracks; ///< Name of copied RecoTrack array in DataStore
+      
+      const std::tuple<double, double> invMassRangeKshort; ///< Invariant mass range for K-short hypothesis
+      const std::tuple<double, double> invMassRangeLambda; ///< Invariant mass range for Lambda hypothesis
+      const std::tuple<double, double> invMassRangePhoton; ///< Invariant mass range for photon hypothesis
+      const double beamPipeRadius; ///< Radius defining inside/outside beam pipe boundary
+      const double vertexChi2CutOutside; ///< Chi2 cut for vertices outside beam pipe
+
+      const int v0FitterMode; ///< V0 fitter mode (0: store at first fit, 1: remove inner hits, 2: mode 1 + SVD restrictions)
+      const bool forcestore; ///< Force store V0 regardless of inner hits (true for mode 0)
+      const bool useOnlyOneSVDHitPair; ///< Use only one SVD hit pair restriction (false for mode 2)
+      const bool enableValidation; ///< Enable validation vertex creation
     };
 
     class Storage {

```

</details>

</blockquote>

Now I'll add documentation for the Storage class: 

<blockquote>

Edited `tracking/v0Finding/fitter/include/NewV0Fitter.h`

<details>

```diff
--- a/tracking/v0Finding/fitter/include/NewV0Fitter.h
+++ b/tracking/v0Finding/fitter/include/NewV0Fitter.h
@@ -87,22 +87,33 @@
       const bool enableValidation; ///< Enable validation vertex creation
     };
 
+    /**
+     * @class Storage
+     * @brief Static storage manager for DataStore arrays used in V0 fitting.
+     * 
+     * This class manages access to various DataStore arrays required for V0 fitting,
+     * including TrackFitResults, V0s, validation vertices, and RecoTracks.
+     */
     class Storage {
     public:
+      /**
+       * @brief Initialize storage arrays based on configuration.
+       * @param config Configuration containing array names and settings
+       */
       static void init(const Config& config);
       
-      static StoreArray<TrackFitResult>* TFResults;
-      static StoreArray<Belle2::V0>* V0s;
-      static std::optional<StoreArray<V0ValidationVertex> *> V0ValidationVertices;
-      static const StoreArray<RecoTrack>* RecoTracks;
-      static StoreArray<RecoTrack>* CopiedRecoTracks;
+      static StoreArray<TrackFitResult>* TFResults; ///< Pointer to TrackFitResult array
+      static StoreArray<Belle2::V0>* V0s; ///< Pointer to V0 array
+      static std::optional<StoreArray<V0ValidationVertex> *> V0ValidationVertices; ///< Pointer to V0ValidationVertex array (optional)
+      static const StoreArray<RecoTrack>* RecoTracks; ///< Pointer to RecoTrack array (read-only)
+      static StoreArray<RecoTrack>* CopiedRecoTracks; ///< Pointer to copied RecoTrack array
 
     private:
-      static StoreArray<TrackFitResult> s_arrayTFResults;
-      static StoreArray<Belle2::V0> s_arrayV0s;
-      static StoreArray<V0ValidationVertex> s_arrayV0ValidationVertices;
-      static StoreArray<RecoTrack> s_arrayRecoTracks;
-      static StoreArray<RecoTrack> s_arrayCopiedRecoTracks;
+      static StoreArray<TrackFitResult> s_arrayTFResults; ///< Static TrackFitResult array
+      static StoreArray<Belle2::V0> s_arrayV0s; ///< Static V0 array
+      static StoreArray<V0ValidationVertex> s_arrayV0ValidationVertices; ///< Static V0ValidationVertex array
+      static StoreArray<RecoTrack> s_arrayRecoTracks; ///< Static RecoTrack array
+      static StoreArray<RecoTrack> s_arrayCopiedRecoTracks; ///< Static copied RecoTrack array
     };
 
     class Track;

```

</details>

</blockquote>

Now I'll add documentation for the forward declarations and the Fitter class: 

<blockquote>

Edited `tracking/v0Finding/fitter/include/NewV0Fitter.h`

<details>

```diff
--- a/tracking/v0Finding/fitter/include/NewV0Fitter.h
+++ b/tracking/v0Finding/fitter/include/NewV0Fitter.h
@@ -116,26 +116,60 @@
       static StoreArray<RecoTrack> s_arrayCopiedRecoTracks; ///< Static copied RecoTrack array
     };
 
+    /// Forward declaration of Track class
     class Track;
+    /// Forward declaration of V0 class
     class V0;
 
+    /**
+     * @class Fitter
+     * @brief Main V0 fitting class for creating V0 candidates from track pairs.
+     * 
+     * This class performs vertex fitting of positive and negative track pairs to create V0 candidates.
+     * It supports different fitting modes and can handle hit removal for improved vertex resolution.
+     */
     class Fitter {
     public:
+      /**
+       * @brief Constructor for the V0 Fitter.
+       * @param config Configuration parameters for the fitter
+       */
       Fitter(const Config& config);
 
+      /**
+       * @brief Fit V0 vertex and store if successful.
+       * @param trackPlus Positive track candidate
+       * @param trackMinus Negative track candidate
+       * @param v0Hypothesis V0 particle hypothesis for fitting
+       * @param isForceStored Output flag indicating if V0 was force-stored
+       * @param isHitRemoved Output flag indicating if hits were removed during fitting
+       * @return True if fitting and storage was successful
+       */
       bool fitAndStore(const Belle2::Track* trackPlus,
                        const Belle2::Track* trackMinus,
                        const Const::ParticleType& v0Hypothesis, bool& isForceStored,
                        bool& isHitRemoved);
 
+      /**
+       * @brief Get daughter track hypotheses for a given V0 hypothesis.
+       * @param v0Hypothesis V0 particle hypothesis
+       * @return Pair of daughter particle hypotheses
+       */
       static std::pair<Const::ParticleType, Const::ParticleType>
       getTrackHypotheses(const Const::ParticleType& v0Hypothesis);
     
     private:
+      /**
+       * @brief Fit vertex and apply pre-selection cuts.
+       * @param v0 V0 object to be fitted
+       * @param trackPlus Positive track candidate
+       * @param trackMinus Negative track candidate
+       * @return True if vertex fit and cuts passed
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

Now I'll add documentation for the V0 class: 

<blockquote>

Edited `tracking/v0Finding/fitter/include/NewV0Fitter.h`

<details>

```diff
--- a/tracking/v0Finding/fitter/include/NewV0Fitter.h
+++ b/tracking/v0Finding/fitter/include/NewV0Fitter.h
@@ -172,31 +172,104 @@
       genfit::GFRaveVertexFactory m_GFRaveVertexFactory; ///< Vertex factory for fitting
     };
 
+    /**
+     * @class V0
+     * @brief V0 candidate class for vertex fitting and validation.
+     * 
+     * This class represents a V0 candidate with fitted vertex information, invariant mass cuts,
+     * and methods for storing V0 objects to the DataStore.
+     */
     class V0 {
       friend Fitter::Fitter(const Config& config);
     public:
+      /// Type alias for V0 object pair (V0 and validation vertex)
       typedef std::pair <
         std::unique_ptr<Belle2::V0>,
         std::unique_ptr<Belle2::V0ValidationVertex>
       > V0ObjPair;
+      /// Type alias for daughter particle pair
       typedef std::pair<Const::ParticleType, Const::ParticleType> DaughterPair;
 
-      bool invMassCut(const uint tolerance = 0) const;      
+      /**
+       * @brief Apply invariant mass cut with optional tolerance.
+       * @param tolerance Tolerance level for mass cut (0=strict, higher=more tolerant)
+       * @return True if mass cut passed
+       */
+      bool invMassCut(const uint tolerance = 0) const;
+
+      /**
+       * @brief Temporarily store V0 object pair for later processing.
+       * @param v0ObjPair V0 object pair to stash
+       */
       void stash(V0ObjPair&& v0ObjPair);
+
+      /**
+       * @brief Retrieve and store stashed V0 object pair.
+       * @return True if stash was successfully popped and stored
+       */
       bool stashPop();
+
+      /**
+       * @brief Get daughter particle hypotheses.
+       * @return Reference to daughter particle pair
+       */
       const DaughterPair &getDaughterParticles() const;
+
+      /**
+       * @brief Get fitted vertex position.
+       * @return Reference to vertex position vector
+       */
       const ROOT::Math::XYZVector& getVertexPos() const;
+
+      /**
+       * @brief Get GFRave vertex object.
+       * @return Reference to GFRave vertex
+       */
       const genfit::GFRaveVertex &getGFRaveVertex() const;
+
+      /**
+       * @brief Set vertex information from GFRave vertex.
+       * @param gfRaveVertex GFRave vertex object
+       */
       void setVertex(genfit::GFRaveVertex &gfRaveVertex);
+
+      /**
+       * @brief Set Lorentz vector at vertex.
+       * @param lorentzVectorAtVertex Lorentz vector to set
+       */
       void setLorentzVectorAtVertex(ROOT::Math::PxPyPzMVector &&lorentzVectorAtVertex);
       
+      /**
+       * @brief Store V0 object pair to DataStore.
+       * @param v0ObjPair V0 object pair to store
+       */
       static void store(V0ObjPair&& v0ObjPair);
+
+      /**
+       * @brief Build V0 object pair from fitted V0 and tracks.
+       * @param v0 Fitted V0 object
+       * @param trackPlus Positive track
+       * @param trackMinus Negative track
+       * @return V0 object pair ready for storage
+       */
       static V0ObjPair buildObj(
         const V0& v0, 
         const Track& trackPlus,
         const Track& trackMinus
       );
+
+      /**
+       * @brief Create V0 object for given hypothesis.
+       * @param v0Hypothesis V0 particle hypothesis
+       * @return V0 object configured for hypothesis
+       */
       static V0 create(const Const::ParticleType& v0Hypothesis);
+
+      /**
+       * @brief Get daughter particles for V0 hypothesis.
+       * @param v0Hypothesis V0 particle hypothesis
+       * @return Daughter particle pair
+       */
       static const DaughterPair getDaughterParticles(const Const::ParticleType& v0Hypothesis);
       
     private:  

```

</details>

</blockquote>

Now I'll add documentation for the private section of the V0 class: 

<blockquote>

Edited `tracking/v0Finding/fitter/include/NewV0Fitter.h`

<details>

```diff
--- a/tracking/v0Finding/fitter/include/NewV0Fitter.h
+++ b/tracking/v0Finding/fitter/include/NewV0Fitter.h
@@ -272,34 +272,52 @@
        */
       static const DaughterPair getDaughterParticles(const Const::ParticleType& v0Hypothesis);
       
-    private:  
+    private:
+      /**
+       * @struct V0ObjStash
+       * @brief Temporary storage for V0 object pairs.
+       * 
+       * This structure holds V0 and validation vertex objects temporarily
+       * before they are stored to the DataStore.
+       */
       struct V0ObjStash {
-        std::unique_ptr<Belle2::V0> m_V0;
-        std::unique_ptr<V0ValidationVertex> m_ValidationV0;
+        std::unique_ptr<Belle2::V0> m_V0; ///< Stored V0 object
+        std::unique_ptr<V0ValidationVertex> m_ValidationV0; ///< Stored validation vertex object
+        
+        /**
+         * @brief Constructor from V0 object pair.
+         * @param v0ObjPair V0 object pair to store
+         */
         V0ObjStash(V0ObjPair&& v0ObjPair) : 
           m_V0(std::move(std::get<0>(v0ObjPair))),
           m_ValidationV0(std::move(std::get<1>(v0ObjPair)))
         {}
       };
       
+      /**
+       * @brief Private constructor for V0 object.
+       * @param V0Particle V0 particle type
+       * @param V0InvMassRange Invariant mass range for cuts
+       * @param daughterParticles Daughter particle hypotheses
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
+      std::optional<V0ObjStash> m_v0ObjStash; ///< Optional stash for V0 object pairs
+      
+      const Const::ParticleType& m_V0Particle; ///< V0 particle type
+      const std::tuple<double, double>& m_V0InvMassRange; ///< Invariant mass range for cuts
+      const DaughterPair& m_daughterParticles; ///< Daughter particle hypotheses
+      genfit::GFRaveVertex m_GFRaveVertex; ///< Fitted vertex object
+      ROOT::Math::XYZVector m_vertexPos; ///< Fitted vertex position
+      ROOT::Math::PxPyPzMVector m_lorentzVectorAtVertex; ///< Lorentz vector at vertex
+
+      static const Config* s_config; ///< Static configuration pointer
+      static const DaughterPair KshortDaughters; ///< K-short daughter particles
+      static const DaughterPair PhotonDaughters; ///< Photon daughter particles
+      static const DaughterPair LambdaDaughters; ///< Lambda daughter particles
+      static const DaughterPair AntiLambdaDaughters; ///< Anti-Lambda daughter particles
     };
 
     class TrackContainer {

```

</details>

</blockquote>

Now I'll add documentation for the TrackContainer class: 

<blockquote>

Edited `tracking/v0Finding/fitter/include/NewV0Fitter.h`

<details>

```diff
--- a/tracking/v0Finding/fitter/include/NewV0Fitter.h
+++ b/tracking/v0Finding/fitter/include/NewV0Fitter.h
@@ -320,34 +320,80 @@
       static const DaughterPair AntiLambdaDaughters; ///< Anti-Lambda daughter particles
     };
 
+    /**
+     * @class TrackContainer
+     * @brief Container class for track-related objects and operations.
+     * 
+     * This class manages RecoTrack objects and their associated genfit::Track objects,
+     * providing methods for track fitting, hit management, and state extraction.
+     */
     class TrackContainer {
       friend Track;
     public:
+      /**
+       * @brief Constructor for TrackContainer.
+       * @param recoTrack RecoTrack object to manage
+       * @param pdgClosest PDG code of closest particle hypothesis
+       * @param isOriginal Flag indicating if this is the original track (not a copy)
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
+       * @brief Prepare track for vertex fitting.
+       * @return True if preparation successful
+       */
       bool prepareVertexFit();
+
+      /**
+       * @brief Perform track fitting.
+       * @return True if fitting successful
+       */
       bool fit();
+
+      /**
+       * @brief Get hit information from track.
+       * @return Vector of hit information pointers
+       */
       const std::vector<RecoHitInformation*> getHitInfos() const;
+
+      /**
+       * @brief Get measured state on plane from first hit.
+       * @return Measured state on plane
+       */
       genfit::MeasuredStateOnPlane getMSOPfromFirstHit() const;
+
+      /**
+       * @brief Get measured state on plane from specific hit.
+       * @param hitInfo Hit information to extract state from
+       * @return Measured state on plane
+       */
       genfit::MeasuredStateOnPlane getMSOPfromHit(const RecoHitInformation* hitInfo) const;
       
+      /**
+       * @brief Create a copy of the track container.
+       * @param trackContainer Source track container to copy
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
+      const genfit::AbsTrackRep* m_trackRep = nullptr; ///< Track representation used for fitting
+      RecoTrack* m_recoTrack = nullptr; ///< Associated RecoTrack object
+      genfit::Track &m_genfitTrack; ///< Reference to genfit track
+      const uint m_OriginalCardinalRepID = 0; ///< Original cardinal representation ID
+      const uint m_pdgClosest = 0; ///< PDG code of closest particle hypothesis
+      const Const::ChargedStable m_particleClosest; ///< Closest particle hypothesis
+      const bool m_isOriginal = true; ///< Flag indicating if this is original track
+      bool m_isSmudged = false; ///< Flag indicating if track has been modified
     };
 
     class Track {

```

</details>

</blockquote>

Finally, I'll add documentation for the Track class: 

<blockquote>

Edited `tracking/v0Finding/fitter/include/NewV0Fitter.h`

<details>

```diff
--- a/tracking/v0Finding/fitter/include/NewV0Fitter.h
+++ b/tracking/v0Finding/fitter/include/NewV0Fitter.h
@@ -396,32 +396,116 @@
       bool m_isSmudged = false; ///< Flag indicating if track has been modified
     };
 
+    /**
+     * @class Track
+     * @brief Track wrapper class for V0 fitting operations.
+     * 
+     * This class wraps Belle2::Track objects and provides methods for V0 fitting,
+     * hit management, track refitting, and consistency checks.
+     */
     class Track {
       friend Fitter::Fitter(const Config&);
-    public:  
+    public:
+      /**
+       * @brief Constructor for Track.
+       * @param isTrackPlus Flag indicating if this is the positive track
+       * @param track Belle2::Track object to wrap
+       * @param particleHyp Particle hypothesis for track
+       */
       Track(
         const bool isTrackPlus,
         const Belle2::Track* track, 
         const Const::ChargedStable& particleHyp
       );
       
+      /**
+       * @brief Prepare track for vertex fitting.
+       * @return True if preparation successful
+       */
       bool prepareVertexFit();
+
+      /**
+       * @brief Check for hits inside vertex position.
+       * @param vertexPos Vertex position to check against
+       * @return True if check completed successfully
+       */
       bool checkInnerHits(const ROOT::Math::XYZVector& vertexPos);
+
+      /**
+       * @brief Check hit consistency for track.
+       */
       void checkHitConsistency();
+
+      /**
+       * @brief Remove inner hits and refit track.
+       * @param vertexPos Vertex position to determine inner hits
+       * @return True if operation successful
+       */
       bool removeInnerHitsAndRefit(const ROOT::Math::XYZVector& vertexPos);
+
+      /**
+       * @brief Copy track and refit.
+       * @return True if operation successful
+       */
       bool copyAndRefit();
+
+      /**
+       * @brief Check if track has been refitted.
+       * @return True if track was refitted
+       */
       bool isRefit() const;
+
+      /**
+       * @brief Check if track has inner hits.
+       * @return True if track has inner hits
+       */
       bool hasInnerHit() const;
+
+      /**
+       * @brief Check if track has hit inconsistencies.
+       * @return True if track has inconsistencies
+       */
       bool hasHitInconsistency() const;
+
+      /**
+       * @brief Get underlying Belle2::Track object.
+       * @return Pointer to Belle2::Track
+       */
       const Belle2::Track *getTrack() const;
+
+      /**
+       * @brief Get genfit::Track object.
+       * @return Pointer to genfit::Track
+       */
       genfit::Track *getGFTrack();
+
+      /**
+       * @brief Compute Lorentz vector at vertex.
+       * @param trackParamsAtVertex Track parameters at vertex
+       * @return Lorentz vector at vertex
+       */
       ROOT::Math::PxPyPzMVector computeLorentzVectorAtVertex(
         const genfit::GFRaveTrackParameters* trackParamsAtVertex
       ) const;
+
+      /**
+       * @brief Create and store TrackFitResult.
+       * @param Bz Magnetic field in z-direction
+       * @param sharedInnermostClusterFlag Flag for shared innermost clusters
+       * @return Pointer to created TrackFitResult
+       */
       const TrackFitResult* storeTrackFitResult(
         const double Bz,
         const unsigned short sharedInnermostClusterFlag
       ) const;
+
+      /**
+       * @brief Check for shared innermost clusters between track pairs.
+       * @param trackPlus Positive track
+       * @param trackMinus Negative track
+       * @param sharedInnermostClusterFlag Output flag for shared clusters
+       * @return True if check completed successfully
+       */
       static bool checkSharedInnermostCluster(
         const Track& trackPlus, 
         const Track& trackMinus,
@@ -429,15 +513,15 @@
       );
 
   private:
-    const Const::ChargedStable m_particleHyp;
-    std::unique_ptr<TrackContainer> m_trackContainer;
-    const Belle2::Track* m_track;
-    uint m_nRemoved = 0;
-    const bool m_isTrackPlus = true;
-    bool m_hasHitInconsistency = false;
-    bool m_hasInnerHit = false;
+    const Const::ChargedStable m_particleHyp; ///< Particle hypothesis for track
+    std::unique_ptr<TrackContainer> m_trackContainer; ///< Track container managing track objects
+    const Belle2::Track* m_track; ///< Underlying Belle2::Track object
+    uint m_nRemoved = 0; ///< Number of removed hits
+    const bool m_isTrackPlus = true; ///< Flag indicating if this is positive track
+    bool m_hasHitInconsistency = false; ///< Flag for hit inconsistencies
+    bool m_hasInnerHit = false; ///< Flag for inner hits
     
-    static const Config* s_config;
+    static const Config* s_config; ///< Static configuration pointer
     };
   }
 }

```

</details>

</blockquote>

Now let me check the final result to ensure all documentation has been added correctly: 

<blockquote>

Read `tracking/v0Finding/fitter/include/NewV0Fitter.h`, lines 30 to 50

</blockquote>

Let me also verify the end of the file looks correct: 

<blockquote>

Read `tracking/v0Finding/fitter/include/NewV0Fitter.h`, lines 520 to 528

</blockquote>

Perfect! I have successfully added comprehensive Doxygen-style documentation to the `tracking/v0Finding/fitter/include/NewV0Fitter.h` file. Here's a summary of what was added:

## Documentation Added:

### 1. **Config struct**
- Class documentation with `@class` and `@brief`
- Constructor documentation with all parameters documented using `@param`
- Inline documentation for all member variables using `///<`

### 2. **Storage class**
- Class documentation explaining its role as a static storage manager
- Method documentation with `@brief` and `@param`
- Inline documentation for all static member variables

### 3. **Fitter class**
- Class documentation describing its main purpose
- Constructor and method documentation with `@brief`, `@param`, and `@return`
- Inline documentation for private members

### 4. **V0 class**
- Comprehensive class documentation
- Documentation for all public methods including `@brief`, `@param`, and `@return`
- Documentation for the nested `V0ObjStash` struct
- Private constructor and member documentation
- Inline documentation for all data members

### 5. **TrackContainer class**
- Class documentation explaining its container role
- Documentation for constructor and destructor
- Documentation for all private methods
- Inline documentation for all member variables

### 6. **Track class**
- Comprehensive class documentation
- Documentation for all public methods with appropriate tags
- Static method documentation
- Inline documentation for all private members

## Key Features of the Documentation:

- **Complete coverage**: Every class, struct, method, and data member is documented
- **Proper Doxygen format**: Uses `@class`, `@brief`, `@param`, `@return`, and `///<` as requested
- **Meaningful descriptions**: Based on the implementation in `NewV0Fitter.cc` and reference to `V0Fitter.h`
- **Consistent style**: All documentation follows the same format and style
- **No code changes**: Only added comments, no existing code was modified or removed

The documentation is now complete and provides comprehensive information about the NewV0Fitter's functionality, parameters, and usage for developers working with this V0 fitting framework.

`(4 min, 7.628 s)`

</blockquote>

<blockquote>

##### Edited Files:

`tracking/v0Finding/fitter/include/NewV0Fitter.h`:

<details>

<summary>Squashed changes (short)</summary>

```diff
--- a/tracking/v0Finding/fitter/include/NewV0Fitter.h
+++ b/tracking/v0Finding/fitter/include/NewV0Fitter.h
@@ -34,8 +34,30 @@
 namespace Belle2 {
   namespace NewV0Fitter {
 
+    /**
+     * @class Config
+     * @brief Configuration structure for the NewV0Fitter containing all necessary parameters and settings.
+     * 
+     * This structure holds all configuration parameters needed for V0 fitting, including fitter modes,
+     * validation settings, mass ranges, and DataStore array names.
+     */
     struct Config {
     public:
+      /**
+       * @brief Constructor for the Config structure.
+       * @param v0FitterMode V0 fitter mode (0: store at first fit, 1: remove inner hits, 2: mode 1 + SVD restrictions)
+       * @param enableValidation Whether to enable validation vertex creation
+       * @param beamPipeRadius Radius defining inside/outside beam pipe boundary
+       * @param vertexChi2CutOutside Chi2 cut for vertices outside beam pipe
+       * @param invMassRangeKshort Invariant mass range for K-short hypothesis
+       * @param invMassRangeLambda Invariant mass range for Lambda hypothesis
+       * @param invMassRangePhoton Invariant mass range for photon hypothesis
+       * @param arrayNameTFResults Name of TrackFitResult array in DataStore
+       * @param arrayNameV0s Name of V0 array in DataStore
+       * @param arrayNameV0ValidationVertices Name of V0ValidationVertex array in DataStore
+       * @param arrayNameRecoTracks Name of RecoTrack array in DataStore
+       * @param arrayNameCopiedRecoTracks Name of copied RecoTrack array in DataStore
+       */
       Config(int v0FitterMode, bool enableValidation, double beamPipeRadius,
              double vertexChi2CutOutside,
              const std::tuple<double, double>& invMassRangeKshort,
@@ -47,177 +69,443 @@
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
+      const std::string arrayNameV0ValidationVertices; ///< Name of V0ValidationVertex array in DataStore
+      const std::string arrayNameRecoTracks; ///< Name of RecoTrack array in DataStore
+      const std::string arrayNameCopiedRecoTracks; ///< Name of copied RecoTrack array in DataStore
+      
+      const std::tuple<double, double> invMassRangeKshort; ///< Invariant mass range for K-short hypothesis
+      const std::tuple<double, double> invMassRangeLambda; ///< Invariant mass range for Lambda hypothesis
+      const std::tuple<double, double> invMassRangePhoton; ///< Invariant mass range for photon hypothesis
+      const double beamPipeRadius; ///< Radius defining inside/outside beam pipe boundary
+      const double vertexChi2CutOutside; ///< Chi2 cut for vertices outside beam pipe
+
+      const int v0FitterMode; ///< V0 fitter mode (0: store at first fit, 1: remove inner hits, 2: mode 1 + SVD restrictions)
+      const bool forcestore; ///< Force store V0 regardless of inner hits (true for mode 0)
+      const bool useOnlyOneSVDHitPair; ///< Use only one SVD hit pair restriction (false for mode 2)
+      const bool enableValidation; ///< Enable validation vertex creation
     };
 
+    /**
+     * @class Storage
+     * @brief Static storage manager for DataStore arrays used in V0 fitting.
+     * 
+     * This class manages access to various DataStore arrays required for V0 fitting,
+     * including TrackFitResults, V0s, validation vertices, and RecoTracks.
+     */
     class Storage {
     public:
+      /**
+       * @brief Initialize storage arrays based on configuration.
+       * @param config Configuration containing array names and settings
+       */
       static void init(const Config& config);
       
-      static StoreArray<TrackFitResult>* TFResults;
-      static StoreArray<Belle2::V0>* V0s;
-      static std::optional<StoreArray<V0ValidationVertex> *> V0ValidationVertices;
-      static const StoreArray<RecoTrack>* RecoTracks;
-      static StoreArray<RecoTrack>* CopiedRecoTracks;
+      static StoreArray<TrackFitResult>* TFResults; ///< Pointer to TrackFitResult array
+      static StoreArray<Belle2::V0>* V0s; ///< Pointer to V0 array
+      static std::optional<StoreArray<V0ValidationVertex> *> V0ValidationVertices; ///< Pointer to V0ValidationVertex array (optional)
+      static const StoreArray<RecoTrack>* RecoTracks; ///< Pointer to RecoTrack array (read-only)
+      static StoreArray<RecoTrack>* CopiedRecoTracks; ///< Pointer to copied RecoTrack array
 
     private:
-      static StoreArray<TrackFitResult> s_arrayTFResults;
-      static StoreArray<Belle2::V0> s_arrayV0s;
-      static StoreArray<V0ValidationVertex> s_arrayV0ValidationVertices;
-      static StoreArray<RecoTrack> s_arrayRecoTracks;
-      static StoreArray<RecoTrack> s_arrayCopiedRecoTracks;
+      static StoreArray<TrackFitResult> s_arrayTFResults; ///< Static TrackFitResult array
+      static StoreArray<Belle2::V0> s_arrayV0s; ///< Static V0 array
+      static StoreArray<V0ValidationVertex> s_arrayV0ValidationVertices; ///< Static V0ValidationVertex array
+      static StoreArray<RecoTrack> s_arrayRecoTracks; ///< Static RecoTrack array
+      static StoreArray<RecoTrack> s_arrayCopiedRecoTracks; ///< Static copied RecoTrack array
     };
 
+    /// Forward declaration of Track class
     class Track;
+    /// Forward declaration of V0 class
     class V0;
 
+    /**
+     * @class Fitter
+     * @brief Main V0 fitting class for creating V0 candidates from track pairs.
+     * 
+     * This class performs vertex fitting of positive and negative track pairs to create V0 candidates.
+     * It supports different fitting modes and can handle hit removal for improved vertex resolution.
+     */
     class Fitter {
     public:
+      /**
+       * @brief Constructor for the V0 Fitter.
+       * @param config Configuration parameters for the fitter
+       */
       Fitter(const Config& config);
 
+      /**
+       * @brief Fit V0 vertex and store if successful.
+       * @param trackPlus Positive track candidate
+       * @param trackMinus Negative track candidate
+       * @param v0Hypothesis V0 particle hypothesis for fitting
+       * @param isForceStored Output flag indicating if V0 was force-stored
+       * @param isHitRemoved Output flag indicating if hits were removed during fitting
+       * @return True if fitting and storage was successful
+       */
       bool fitAndStore(const Belle2::Track* trackPlus,
                        const Belle2::Track* trackMinus,
                        const Const::ParticleType& v0Hypothesis, bool& isForceStored,
                        bool& isHitRemoved);
 
+      /**
+       * @brief Get daughter track hypotheses for a given V0 hypothesis.
+       * @param v0Hypothesis V0 particle hypothesis
+       * @return Pair of daughter particle hypotheses
+       */
       static std::pair<Const::ParticleType, Const::ParticleType>
       getTrackHypotheses(const Const::ParticleType& v0Hypothesis);
     
     private:
+      /**
+       * @brief Fit vertex and apply pre-selection cuts.
+       * @param v0 V0 object to be fitted
+       * @param trackPlus Positive track candidate
+       * @param trackMinus Negative track candidate
+       * @return True if vertex fit and cuts passed
+       */
       bool fitVertexAndPrecut(V0& v0, Track& trackPlus, Track& trackMinus);
       
-      const Config m_config;
-      genfit::GFRaveVertexFactory m_GFRaveVertexFactory;
+      const Config m_config; ///< Configuration parameters
+      genfit::GFRaveVertexFactory m_GFRaveVertexFactory; ///< Vertex factory for fitting
     };
 
+    /**
+     * @class V0
+     * @brief V0 candidate class for vertex fitting and validation.
+     * 
+     * This class represents a V0 candidate with fitted vertex information, invariant mass cuts,
+     * and methods for storing V0 objects to the DataStore.
+     */
     class V0 {
       friend Fitter::Fitter(const Config& config);
     public:
+      /// Type alias for V0 object pair (V0 and validation vertex)
       typedef std::pair <
         std::unique_ptr<Belle2::V0>,
         std::unique_ptr<Belle2::V0ValidationVertex>
       > V0ObjPair;
+      /// Type alias for daughter particle pair
       typedef std::pair<Const::ParticleType, Const::ParticleType> DaughterPair;
 
-      bool invMassCut(const uint tolerance = 0) const;      
+      /**
+       * @brief Apply invariant mass cut with optional tolerance.
+       * @param tolerance Tolerance level for mass cut (0=strict, higher=more tolerant)
+       * @return True if mass cut passed
+       */
+      bool invMassCut(const uint tolerance = 0) const;
+
+      /**
+       * @brief Temporarily store V0 object pair for later processing.
+       * @param v0ObjPair V0 object pair to stash
+       */
       void stash(V0ObjPair&& v0ObjPair);
+
+      /**
+       * @brief Retrieve and store stashed V0 object pair.
+       * @return True if stash was successfully popped and stored
+       */
       bool stashPop();
+
+      /**
+       * @brief Get daughter particle hypotheses.
+       * @return Reference to daughter particle pair
+       */
       const DaughterPair &getDaughterParticles() const;
+
+      /**
+       * @brief Get fitted vertex position.
+       * @return Reference to vertex position vector
+       */
       const ROOT::Math::XYZVector& getVertexPos() const;
+
+      /**
+       * @brief Get GFRave vertex object.
+       * @return Reference to GFRave vertex
+       */
       const genfit::GFRaveVertex &getGFRaveVertex() const;
+
+      /**
+       * @brief Set vertex information from GFRave vertex.
+       * @param gfRaveVertex GFRave vertex object
+       */
       void setVertex(genfit::GFRaveVertex &gfRaveVertex);
+
+      /**
+       * @brief Set Lorentz vector at vertex.
+       * @param lorentzVectorAtVertex Lorentz vector to set
+       */
       void setLorentzVectorAtVertex(ROOT::Math::PxPyPzMVector &&lorentzVectorAtVertex);
       
+      /**
+       * @brief Store V0 object pair to DataStore.
+       * @param v0ObjPair V0 object pair to store
+       */
       static void store(V0ObjPair&& v0ObjPair);
+
+      /**
+       * @brief Build V0 object pair from fitted V0 and tracks.
+       * @param v0 Fitted V0 object
+       * @param trackPlus Positive track
+       * @param trackMinus Negative track
+       * @return V0 object pair ready for storage
+       */
       static V0ObjPair buildObj(
         const V0& v0, 
         const Track& trackPlus,
         const Track& trackMinus
       );
+
+      /**
+       * @brief Create V0 object for given hypothesis.
+       * @param v0Hypothesis V0 particle hypothesis
+       * @return V0 object configured for hypothesis
+       */
       static V0 create(const Const::ParticleType& v0Hypothesis);
+
+      /**
+       * @brief Get daughter particles for V0 hypothesis.
+       * @param v0Hypothesis V0 particle hypothesis
+       * @return Daughter particle pair
+       */
       static const DaughterPair getDaughterParticles(const Const::ParticleType& v0Hypothesis);
       
-    private:  
+    private:
+      /**
+       * @struct V0ObjStash
+       * @brief Temporary storage for V0 object pairs.
+       * 
+       * This structure holds V0 and validation vertex objects temporarily
+       * before they are stored to the DataStore.
+       */
       struct V0ObjStash {
-        std::unique_ptr<Belle2::V0> m_V0;
-        std::unique_ptr<V0ValidationVertex> m_ValidationV0;
+        std::unique_ptr<Belle2::V0> m_V0; ///< Stored V0 object
+        std::unique_ptr<V0ValidationVertex> m_ValidationV0; ///< Stored validation vertex object
+        
+        /**
+         * @brief Constructor from V0 object pair.
+         * @param v0ObjPair V0 object pair to store
+         */
         V0ObjStash(V0ObjPair&& v0ObjPair) : 
           m_V0(std::move(std::get<0>(v0ObjPair))),
           m_ValidationV0(std::move(std::get<1>(v0ObjPair)))
         {}
       };
       
+      /**
+       * @brief Private constructor for V0 object.
+       * @param V0Particle V0 particle type
+       * @param V0InvMassRange Invariant mass range for cuts
+       * @param daughterParticles Daughter particle hypotheses
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
+      std::optional<V0ObjStash> m_v0ObjStash; ///< Optional stash for V0 object pairs
+      
+      const Const::ParticleType& m_V0Particle; ///< V0 particle type
+      const std::tuple<double, double>& m_V0InvMassRange; ///< Invariant mass range for cuts
+      const DaughterPair& m_daughterParticles; ///< Daughter particle hypotheses
+      genfit::GFRaveVertex m_GFRaveVertex; ///< Fitted vertex object
+      ROOT::Math::XYZVector m_vertexPos; ///< Fitted vertex position
+      ROOT::Math::PxPyPzMVector m_lorentzVectorAtVertex; ///< Lorentz vector at vertex
+
+      static const Config* s_config; ///< Static configuration pointer
+      static const DaughterPair KshortDaughters; ///< K-short daughter particles
+      static const DaughterPair PhotonDaughters; ///< Photon daughter particles
+      static const DaughterPair LambdaDaughters; ///< Lambda daughter particles
+      static const DaughterPair AntiLambdaDaughters; ///< Anti-Lambda daughter particles
     };
 
+    /**
+     * @class TrackContainer
+     * @brief Container class for track-related objects and operations.
+     * 
+     * This class manages RecoTrack objects and their associated genfit::Track objects,
+     * providing methods for track fitting, hit management, and state extraction.
+     */
     class TrackContainer {
       friend Track;
     public:
+      /**
+       * @brief Constructor for TrackContainer.
+       * @param recoTrack RecoTrack object to manage
+       * @param pdgClosest PDG code of closest particle hypothesis
+       * @param isOriginal Flag indicating if this is the original track (not a copy)
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
+       * @brief Prepare track for vertex fitting.
+       * @return True if preparation successful
+       */
       bool prepareVertexFit();
+
+      /**
+       * @brief Perform track fitting.
+       * @return True if fitting successful
+       */
       bool fit();
+
+      /**
+       * @brief Get hit information from track.
+       * @return Vector of hit information pointers
+       */
       const std::vector<RecoHitInformation*> getHitInfos() const;
+
+      /**
+       * @brief Get measured state on plane from first hit.
+       * @return Measured state on plane
+       */
       genfit::MeasuredStateOnPlane getMSOPfromFirstHit() const;
+
+      /**
+       * @brief Get measured state on plane from specific hit.
+       * @param hitInfo Hit information to extract state from
+       * @return Measured state on plane
+       */
       genfit::MeasuredStateOnPlane getMSOPfromHit(const RecoHitInformation* hitInfo) const;
       
+      /**
+       * @brief Create a copy of the track container.
+       * @param trackContainer Source track container to copy
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
+      const genfit::AbsTrackRep* m_trackRep = nullptr; ///< Track representation used for fitting
+      RecoTrack* m_recoTrack = nullptr; ///< Associated RecoTrack object
+      genfit::Track &m_genfitTrack; ///< Reference to genfit track
+      const uint m_OriginalCardinalRepID = 0; ///< Original cardinal representation ID
+      const uint m_pdgClosest = 0; ///< PDG code of closest particle hypothesis
+      const Const::ChargedStable m_particleClosest; ///< Closest particle hypothesis
+      const bool m_isOriginal = true; ///< Flag indicating if this is original track
+      bool m_isSmudged = false; ///< Flag indicating if track has been modified
     };
 
+    /**
+     * @class Track
+     * @brief Track wrapper class for V0 fitting operations.
+     * 
+     * This class wraps Belle2::Track objects and provides methods for V0 fitting,
+     * hit management, track refitting, and consistency checks.
+     */
     class Track {
       friend Fitter::Fitter(const Config&);
-    public:  
+    public:
+      /**
+       * @brief Constructor for Track.
+       * @param isTrackPlus Flag indicating if this is the positive track
+       * @param track Belle2::Track object to wrap
+       * @param particleHyp Particle hypothesis for track
+       */
       Track(
         const bool isTrackPlus,
         const Belle2::Track* track, 
         const Const::ChargedStable& particleHyp
       );
       
+      /**
+       * @brief Prepare track for vertex fitting.
+       * @return True if preparation successful
+       */
       bool prepareVertexFit();
+
+      /**
+       * @brief Check for hits inside vertex position.
+       * @param vertexPos Vertex position to check against
+       * @return True if check completed successfully
+       */
       bool checkInnerHits(const ROOT::Math::XYZVector& vertexPos);
+
+      /**
+       * @brief Check hit consistency for track.
+       */
       void checkHitConsistency();
+
+      /**
+       * @brief Remove inner hits and refit track.
+       * @param vertexPos Vertex position to determine inner hits
+       * @return True if operation successful
+       */
       bool removeInnerHitsAndRefit(const ROOT::Math::XYZVector& vertexPos);
+
+      /**
+       * @brief Copy track and refit.
+       * @return True if operation successful
+       */
       bool copyAndRefit();
+
+      /**
+       * @brief Check if track has been refitted.
+       * @return True if track was refitted
+       */
       bool isRefit() const;
+
+      /**
+       * @brief Check if track has inner hits.
+       * @return True if track has inner hits
+       */
       bool hasInnerHit() const;
+
+      /**
+       * @brief Check if track has hit inconsistencies.
+       * @return True if track has inconsistencies
+       */
       bool hasHitInconsistency() const;
+
+      /**
+       * @brief Get underlying Belle2::Track object.
+       * @return Pointer to Belle2::Track
+       */
       const Belle2::Track *getTrack() const;
+
+      /**
+       * @brief Get genfit::Track object.
+       * @return Pointer to genfit::Track
+       */
       genfit::Track *getGFTrack();
+
+      /**
+       * @brief Compute Lorentz vector at vertex.
+       * @param trackParamsAtVertex Track parameters at vertex
+       * @return Lorentz vector at vertex
+       */
       ROOT::Math::PxPyPzMVector computeLorentzVectorAtVertex(
         const genfit::GFRaveTrackParameters* trackParamsAtVertex
       ) const;
+
+      /**
+       * @brief Create and store TrackFitResult.
+       * @param Bz Magnetic field in z-direction
+       * @param sharedInnermostClusterFlag Flag for shared innermost clusters
+       * @return Pointer to created TrackFitResult
+       */
       const TrackFitResult* storeTrackFitResult(
         const double Bz,
         const unsigned short sharedInnermostClusterFlag
       ) const;
+
+      /**
+       * @brief Check for shared innermost clusters between track pairs.
+       * @param trackPlus Positive track
+       * @param trackMinus Negative track
+       * @param sharedInnermostClusterFlag Output flag for shared clusters
+       * @return True if check completed successfully
+       */
       static bool checkSharedInnermostCluster(
         const Track& trackPlus, 
         const Track& trackMinus,
@@ -225,15 +513,15 @@
       );
 
   private:
-    const Const::ChargedStable m_particleHyp;
-    std::unique_ptr<TrackContainer> m_trackContainer;
-    const Belle2::Track* m_track;
-    uint m_nRemoved = 0;
-    const bool m_isTrackPlus = true;
-    bool m_hasHitInconsistency = false;
-    bool m_hasInnerHit = false;
+    const Const::ChargedStable m_particleHyp; ///< Particle hypothesis for track
+    std::unique_ptr<TrackContainer> m_trackContainer; ///< Track container managing track objects
+    const Belle2::Track* m_track; ///< Underlying Belle2::Track object
+    uint m_nRemoved = 0; ///< Number of removed hits
+    const bool m_isTrackPlus = true; ///< Flag indicating if this is positive track
+    bool m_hasHitInconsistency = false; ///< Flag for hit inconsistencies
+    bool m_hasInnerHit = false; ///< Flag for inner hits
     
-    static const Config* s_config;
+    static const Config* s_config; ///< Static configuration pointer
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
@@ -1,240 +1,528 @@
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
 
 namespace Belle2 {
   namespace NewV0Fitter {
 
+    /**
+     * @class Config
+     * @brief Configuration structure for the NewV0Fitter containing all necessary parameters and settings.
+     * 
+     * This structure holds all configuration parameters needed for V0 fitting, including fitter modes,
+     * validation settings, mass ranges, and DataStore array names.
+     */
     struct Config {
     public:
+      /**
+       * @brief Constructor for the Config structure.
+       * @param v0FitterMode V0 fitter mode (0: store at first fit, 1: remove inner hits, 2: mode 1 + SVD restrictions)
+       * @param enableValidation Whether to enable validation vertex creation
+       * @param beamPipeRadius Radius defining inside/outside beam pipe boundary
+       * @param vertexChi2CutOutside Chi2 cut for vertices outside beam pipe
+       * @param invMassRangeKshort Invariant mass range for K-short hypothesis
+       * @param invMassRangeLambda Invariant mass range for Lambda hypothesis
+       * @param invMassRangePhoton Invariant mass range for photon hypothesis
+       * @param arrayNameTFResults Name of TrackFitResult array in DataStore
+       * @param arrayNameV0s Name of V0 array in DataStore
+       * @param arrayNameV0ValidationVertices Name of V0ValidationVertex array in DataStore
+       * @param arrayNameRecoTracks Name of RecoTrack array in DataStore
+       * @param arrayNameCopiedRecoTracks Name of copied RecoTrack array in DataStore
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
+      const std::string arrayNameV0ValidationVertices; ///< Name of V0ValidationVertex array in DataStore
+      const std::string arrayNameRecoTracks; ///< Name of RecoTrack array in DataStore
+      const std::string arrayNameCopiedRecoTracks; ///< Name of copied RecoTrack array in DataStore
+      
+      const std::tuple<double, double> invMassRangeKshort; ///< Invariant mass range for K-short hypothesis
+      const std::tuple<double, double> invMassRangeLambda; ///< Invariant mass range for Lambda hypothesis
+      const std::tuple<double, double> invMassRangePhoton; ///< Invariant mass range for photon hypothesis
+      const double beamPipeRadius; ///< Radius defining inside/outside beam pipe boundary
+      const double vertexChi2CutOutside; ///< Chi2 cut for vertices outside beam pipe
+
+      const int v0FitterMode; ///< V0 fitter mode (0: store at first fit, 1: remove inner hits, 2: mode 1 + SVD restrictions)
+      const bool forcestore; ///< Force store V0 regardless of inner hits (true for mode 0)
+      const bool useOnlyOneSVDHitPair; ///< Use only one SVD hit pair restriction (false for mode 2)
+      const bool enableValidation; ///< Enable validation vertex creation
     };
 
+    /**
+     * @class Storage
+     * @brief Static storage manager for DataStore arrays used in V0 fitting.
+     * 
+     * This class manages access to various DataStore arrays required for V0 fitting,
+     * including TrackFitResults, V0s, validation vertices, and RecoTracks.
+     */
     class Storage {
     public:
+      /**
+       * @brief Initialize storage arrays based on configuration.
+       * @param config Configuration containing array names and settings
+       */
       static void init(const Config& config);
       
-      static StoreArray<TrackFitResult>* TFResults;
-      static StoreArray<Belle2::V0>* V0s;
-      static std::optional<StoreArray<V0ValidationVertex> *> V0ValidationVertices;
-      static const StoreArray<RecoTrack>* RecoTracks;
-      static StoreArray<RecoTrack>* CopiedRecoTracks;
+      static StoreArray<TrackFitResult>* TFResults; ///< Pointer to TrackFitResult array
+      static StoreArray<Belle2::V0>* V0s; ///< Pointer to V0 array
+      static std::optional<StoreArray<V0ValidationVertex> *> V0ValidationVertices; ///< Pointer to V0ValidationVertex array (optional)
+      static const StoreArray<RecoTrack>* RecoTracks; ///< Pointer to RecoTrack array (read-only)
+      static StoreArray<RecoTrack>* CopiedRecoTracks; ///< Pointer to copied RecoTrack array
 
     private:
-      static StoreArray<TrackFitResult> s_arrayTFResults;
-      static StoreArray<Belle2::V0> s_arrayV0s;
-      static StoreArray<V0ValidationVertex> s_arrayV0ValidationVertices;
-      static StoreArray<RecoTrack> s_arrayRecoTracks;
-      static StoreArray<RecoTrack> s_arrayCopiedRecoTracks;
+      static StoreArray<TrackFitResult> s_arrayTFResults; ///< Static TrackFitResult array
+      static StoreArray<Belle2::V0> s_arrayV0s; ///< Static V0 array
+      static StoreArray<V0ValidationVertex> s_arrayV0ValidationVertices; ///< Static V0ValidationVertex array
+      static StoreArray<RecoTrack> s_arrayRecoTracks; ///< Static RecoTrack array
+      static StoreArray<RecoTrack> s_arrayCopiedRecoTracks; ///< Static copied RecoTrack array
     };
 
+    /// Forward declaration of Track class
     class Track;
+    /// Forward declaration of V0 class
     class V0;
 
+    /**
+     * @class Fitter
+     * @brief Main V0 fitting class for creating V0 candidates from track pairs.
+     * 
+     * This class performs vertex fitting of positive and negative track pairs to create V0 candidates.
+     * It supports different fitting modes and can handle hit removal for improved vertex resolution.
+     */
     class Fitter {
     public:
+      /**
+       * @brief Constructor for the V0 Fitter.
+       * @param config Configuration parameters for the fitter
+       */
       Fitter(const Config& config);
 
+      /**
+       * @brief Fit V0 vertex and store if successful.
+       * @param trackPlus Positive track candidate
+       * @param trackMinus Negative track candidate
+       * @param v0Hypothesis V0 particle hypothesis for fitting
+       * @param isForceStored Output flag indicating if V0 was force-stored
+       * @param isHitRemoved Output flag indicating if hits were removed during fitting
+       * @return True if fitting and storage was successful
+       */
       bool fitAndStore(const Belle2::Track* trackPlus,
                        const Belle2::Track* trackMinus,
                        const Const::ParticleType& v0Hypothesis, bool& isForceStored,
                        bool& isHitRemoved);
 
+      /**
+       * @brief Get daughter track hypotheses for a given V0 hypothesis.
+       * @param v0Hypothesis V0 particle hypothesis
+       * @return Pair of daughter particle hypotheses
+       */
       static std::pair<Const::ParticleType, Const::ParticleType>
       getTrackHypotheses(const Const::ParticleType& v0Hypothesis);
     
     private:
+      /**
+       * @brief Fit vertex and apply pre-selection cuts.
+       * @param v0 V0 object to be fitted
+       * @param trackPlus Positive track candidate
+       * @param trackMinus Negative track candidate
+       * @return True if vertex fit and cuts passed
+       */
       bool fitVertexAndPrecut(V0& v0, Track& trackPlus, Track& trackMinus);
       
-      const Config m_config;
-      genfit::GFRaveVertexFactory m_GFRaveVertexFactory;
+      const Config m_config; ///< Configuration parameters
+      genfit::GFRaveVertexFactory m_GFRaveVertexFactory; ///< Vertex factory for fitting
     };
 
+    /**
+     * @class V0
+     * @brief V0 candidate class for vertex fitting and validation.
+     * 
+     * This class represents a V0 candidate with fitted vertex information, invariant mass cuts,
+     * and methods for storing V0 objects to the DataStore.
+     */
     class V0 {
       friend Fitter::Fitter(const Config& config);
     public:
+      /// Type alias for V0 object pair (V0 and validation vertex)
       typedef std::pair <
         std::unique_ptr<Belle2::V0>,
         std::unique_ptr<Belle2::V0ValidationVertex>
       > V0ObjPair;
+      /// Type alias for daughter particle pair
       typedef std::pair<Const::ParticleType, Const::ParticleType> DaughterPair;
 
-      bool invMassCut(const uint tolerance = 0) const;      
+      /**
+       * @brief Apply invariant mass cut with optional tolerance.
+       * @param tolerance Tolerance level for mass cut (0=strict, higher=more tolerant)
+       * @return True if mass cut passed
+       */
+      bool invMassCut(const uint tolerance = 0) const;
+
+      /**
+       * @brief Temporarily store V0 object pair for later processing.
+       * @param v0ObjPair V0 object pair to stash
+       */
       void stash(V0ObjPair&& v0ObjPair);
+
+      /**
+       * @brief Retrieve and store stashed V0 object pair.
+       * @return True if stash was successfully popped and stored
+       */
       bool stashPop();
+
+      /**
+       * @brief Get daughter particle hypotheses.
+       * @return Reference to daughter particle pair
+       */
       const DaughterPair &getDaughterParticles() const;
+
+      /**
+       * @brief Get fitted vertex position.
+       * @return Reference to vertex position vector
+       */
       const ROOT::Math::XYZVector& getVertexPos() const;
+
+      /**
+       * @brief Get GFRave vertex object.
+       * @return Reference to GFRave vertex
+       */
       const genfit::GFRaveVertex &getGFRaveVertex() const;
+
+      /**
+       * @brief Set vertex information from GFRave vertex.
+       * @param gfRaveVertex GFRave vertex object
+       */
       void setVertex(genfit::GFRaveVertex &gfRaveVertex);
+
+      /**
+       * @brief Set Lorentz vector at vertex.
+       * @param lorentzVectorAtVertex Lorentz vector to set
+       */
       void setLorentzVectorAtVertex(ROOT::Math::PxPyPzMVector &&lorentzVectorAtVertex);
       
+      /**
+       * @brief Store V0 object pair to DataStore.
+       * @param v0ObjPair V0 object pair to store
+       */
       static void store(V0ObjPair&& v0ObjPair);
+
+      /**
+       * @brief Build V0 object pair from fitted V0 and tracks.
+       * @param v0 Fitted V0 object
+       * @param trackPlus Positive track
+       * @param trackMinus Negative track
+       * @return V0 object pair ready for storage
+       */
       static V0ObjPair buildObj(
         const V0& v0, 
         const Track& trackPlus,
         const Track& trackMinus
       );
+
+      /**
+       * @brief Create V0 object for given hypothesis.
+       * @param v0Hypothesis V0 particle hypothesis
+       * @return V0 object configured for hypothesis
+       */
       static V0 create(const Const::ParticleType& v0Hypothesis);
+
+      /**
+       * @brief Get daughter particles for V0 hypothesis.
+       * @param v0Hypothesis V0 particle hypothesis
+       * @return Daughter particle pair
+       */
       static const DaughterPair getDaughterParticles(const Const::ParticleType& v0Hypothesis);
       
-    private:  
+    private:
+      /**
+       * @struct V0ObjStash
+       * @brief Temporary storage for V0 object pairs.
+       * 
+       * This structure holds V0 and validation vertex objects temporarily
+       * before they are stored to the DataStore.
+       */
       struct V0ObjStash {
-        std::unique_ptr<Belle2::V0> m_V0;
-        std::unique_ptr<V0ValidationVertex> m_ValidationV0;
+        std::unique_ptr<Belle2::V0> m_V0; ///< Stored V0 object
+        std::unique_ptr<V0ValidationVertex> m_ValidationV0; ///< Stored validation vertex object
+        
+        /**
+         * @brief Constructor from V0 object pair.
+         * @param v0ObjPair V0 object pair to store
+         */
         V0ObjStash(V0ObjPair&& v0ObjPair) : 
           m_V0(std::move(std::get<0>(v0ObjPair))),
           m_ValidationV0(std::move(std::get<1>(v0ObjPair)))
         {}
       };
       
+      /**
+       * @brief Private constructor for V0 object.
+       * @param V0Particle V0 particle type
+       * @param V0InvMassRange Invariant mass range for cuts
+       * @param daughterParticles Daughter particle hypotheses
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
+      std::optional<V0ObjStash> m_v0ObjStash; ///< Optional stash for V0 object pairs
+      
+      const Const::ParticleType& m_V0Particle; ///< V0 particle type
+      const std::tuple<double, double>& m_V0InvMassRange; ///< Invariant mass range for cuts
+      const DaughterPair& m_daughterParticles; ///< Daughter particle hypotheses
+      genfit::GFRaveVertex m_GFRaveVertex; ///< Fitted vertex object
+      ROOT::Math::XYZVector m_vertexPos; ///< Fitted vertex position
+      ROOT::Math::PxPyPzMVector m_lorentzVectorAtVertex; ///< Lorentz vector at vertex
+
+      static const Config* s_config; ///< Static configuration pointer
+      static const DaughterPair KshortDaughters; ///< K-short daughter particles
+      static const DaughterPair PhotonDaughters; ///< Photon daughter particles
+      static const DaughterPair LambdaDaughters; ///< Lambda daughter particles
+      static const DaughterPair AntiLambdaDaughters; ///< Anti-Lambda daughter particles
     };
 
+    /**
+     * @class TrackContainer
+     * @brief Container class for track-related objects and operations.
+     * 
+     * This class manages RecoTrack objects and their associated genfit::Track objects,
+     * providing methods for track fitting, hit management, and state extraction.
+     */
     class TrackContainer {
       friend Track;
     public:
+      /**
+       * @brief Constructor for TrackContainer.
+       * @param recoTrack RecoTrack object to manage
+       * @param pdgClosest PDG code of closest particle hypothesis
+       * @param isOriginal Flag indicating if this is the original track (not a copy)
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
+       * @brief Prepare track for vertex fitting.
+       * @return True if preparation successful
+       */
       bool prepareVertexFit();
+
+      /**
+       * @brief Perform track fitting.
+       * @return True if fitting successful
+       */
       bool fit();
+
+      /**
+       * @brief Get hit information from track.
+       * @return Vector of hit information pointers
+       */
       const std::vector<RecoHitInformation*> getHitInfos() const;
+
+      /**
+       * @brief Get measured state on plane from first hit.
+       * @return Measured state on plane
+       */
       genfit::MeasuredStateOnPlane getMSOPfromFirstHit() const;
+
+      /**
+       * @brief Get measured state on plane from specific hit.
+       * @param hitInfo Hit information to extract state from
+       * @return Measured state on plane
+       */
       genfit::MeasuredStateOnPlane getMSOPfromHit(const RecoHitInformation* hitInfo) const;
       
+      /**
+       * @brief Create a copy of the track container.
+       * @param trackContainer Source track container to copy
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
+      const genfit::AbsTrackRep* m_trackRep = nullptr; ///< Track representation used for fitting
+      RecoTrack* m_recoTrack = nullptr; ///< Associated RecoTrack object
+      genfit::Track &m_genfitTrack; ///< Reference to genfit track
+      const uint m_OriginalCardinalRepID = 0; ///< Original cardinal representation ID
+      const uint m_pdgClosest = 0; ///< PDG code of closest particle hypothesis
+      const Const::ChargedStable m_particleClosest; ///< Closest particle hypothesis
+      const bool m_isOriginal = true; ///< Flag indicating if this is original track
+      bool m_isSmudged = false; ///< Flag indicating if track has been modified
     };
 
+    /**
+     * @class Track
+     * @brief Track wrapper class for V0 fitting operations.
+     * 
+     * This class wraps Belle2::Track objects and provides methods for V0 fitting,
+     * hit management, track refitting, and consistency checks.
+     */
     class Track {
       friend Fitter::Fitter(const Config&);
-    public:  
+    public:
+      /**
+       * @brief Constructor for Track.
+       * @param isTrackPlus Flag indicating if this is the positive track
+       * @param track Belle2::Track object to wrap
+       * @param particleHyp Particle hypothesis for track
+       */
       Track(
         const bool isTrackPlus,
         const Belle2::Track* track, 
         const Const::ChargedStable& particleHyp
       );
       
+      /**
+       * @brief Prepare track for vertex fitting.
+       * @return True if preparation successful
+       */
       bool prepareVertexFit();
+
+      /**
+       * @brief Check for hits inside vertex position.
+       * @param vertexPos Vertex position to check against
+       * @return True if check completed successfully
+       */
       bool checkInnerHits(const ROOT::Math::XYZVector& vertexPos);
+
+      /**
+       * @brief Check hit consistency for track.
+       */
       void checkHitConsistency();
+
+      /**
+       * @brief Remove inner hits and refit track.
+       * @param vertexPos Vertex position to determine inner hits
+       * @return True if operation successful
+       */
       bool removeInnerHitsAndRefit(const ROOT::Math::XYZVector& vertexPos);
+
+      /**
+       * @brief Copy track and refit.
+       * @return True if operation successful
+       */
       bool copyAndRefit();
+
+      /**
+       * @brief Check if track has been refitted.
+       * @return True if track was refitted
+       */
       bool isRefit() const;
+
+      /**
+       * @brief Check if track has inner hits.
+       * @return True if track has inner hits
+       */
       bool hasInnerHit() const;
+
+      /**
+       * @brief Check if track has hit inconsistencies.
+       * @return True if track has inconsistencies
+       */
       bool hasHitInconsistency() const;
+
+      /**
+       * @brief Get underlying Belle2::Track object.
+       * @return Pointer to Belle2::Track
+       */
       const Belle2::Track *getTrack() const;
+
+      /**
+       * @brief Get genfit::Track object.
+       * @return Pointer to genfit::Track
+       */
       genfit::Track *getGFTrack();
+
+      /**
+       * @brief Compute Lorentz vector at vertex.
+       * @param trackParamsAtVertex Track parameters at vertex
+       * @return Lorentz vector at vertex
+       */
       ROOT::Math::PxPyPzMVector computeLorentzVectorAtVertex(
         const genfit::GFRaveTrackParameters* trackParamsAtVertex
       ) const;
+
+      /**
+       * @brief Create and store TrackFitResult.
+       * @param Bz Magnetic field in z-direction
+       * @param sharedInnermostClusterFlag Flag for shared innermost clusters
+       * @return Pointer to created TrackFitResult
+       */
       const TrackFitResult* storeTrackFitResult(
         const double Bz,
         const unsigned short sharedInnermostClusterFlag
       ) const;
+
+      /**
+       * @brief Check for shared innermost clusters between track pairs.
+       * @param trackPlus Positive track
+       * @param trackMinus Negative track
+       * @param sharedInnermostClusterFlag Output flag for shared clusters
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
+    const Const::ChargedStable m_particleHyp; ///< Particle hypothesis for track
+    std::unique_ptr<TrackContainer> m_trackContainer; ///< Track container managing track objects
+    const Belle2::Track* m_track; ///< Underlying Belle2::Track object
+    uint m_nRemoved = 0; ///< Number of removed hits
+    const bool m_isTrackPlus = true; ///< Flag indicating if this is positive track
+    bool m_hasHitInconsistency = false; ///< Flag for hit inconsistencies
+    bool m_hasInnerHit = false; ///< Flag for inner hits
     
-    static const Config* s_config;
+    static const Config* s_config; ///< Static configuration pointer
     };
   }
 }
 

```

</details>

</blockquote>

