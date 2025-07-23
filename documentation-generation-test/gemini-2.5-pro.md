Document ID: `chat-logs/1522198`

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

#### GitHub Copilot (gemini-2.5-pro):

I will add Doxygen-style documentation to `tracking/v0Finding/fitter/include/NewV0Fitter.h`. I will document all classes, structs, methods, and data members as you requested, using the implementation details from `tracking/v0Finding/fitter/src/NewV0Fitter.cc` and `tracking/v0Finding/fitter/include/V0Fitter.h` for context. 

<blockquote>

Edited `tracking/v0Finding/fitter/include/NewV0Fitter.h`

<details>

```diff
--- a/tracking/v0Finding/fitter/include/NewV0Fitter.h
+++ b/tracking/v0Finding/fitter/include/NewV0Fitter.h
@@ -34,8 +34,27 @@
 namespace Belle2 {
   namespace NewV0Fitter {
 
+    /**
+     * @class Config
+     * @brief Configuration for the NewV0Fitter.
+     */
     struct Config {
     public:
+      /**
+       * @brief Constructor for the Config struct.
+       * @param v0FitterMode The mode for the V0 fitter.
+       * @param enableValidation Flag to enable validation.
+       * @param beamPipeRadius The radius of the beam pipe.
+       * @param vertexChi2CutOutside The chi2 cut for vertices outside the beam pipe.
+       * @param invMassRangeKshort The invariant mass range for K-short.
+       * @param invMassRangeLambda The invariant mass range for Lambda.
+       * @param invMassRangePhoton The invariant mass range for Photon.
+       * @param arrayNameTFResults The name of the TrackFitResult StoreArray.
+       * @param arrayNameV0s The name of the V0 StoreArray.
+       * @param arrayNameV0ValidationVertices The name of the V0ValidationVertex StoreArray.
+       * @param arrayNameRecoTracks The name of the RecoTrack StoreArray.
+       * @param arrayNameCopiedRecoTracks The name of the copied RecoTrack StoreArray.
+       */
       Config(int v0FitterMode, bool enableValidation, double beamPipeRadius,
              double vertexChi2CutOutside,
              const std::tuple<double, double>& invMassRangeKshort,
@@ -47,177 +66,400 @@
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
-    };
-
+      const std::string arrayNameTFResults; ///< Name of the TrackFitResult StoreArray.
+      const std::string arrayNameV0s; ///< Name of the V0 StoreArray.
+      const std::string arrayNameV0ValidationVertices; ///< Name of the V0ValidationVertex StoreArray.
+      const std::string arrayNameRecoTracks; ///< Name of the RecoTrack StoreArray.
+      const std::string arrayNameCopiedRecoTracks; ///< Name of the copied RecoTrack StoreArray.
+      
+      const std::tuple<double, double> invMassRangeKshort; ///< Invariant mass range for K-short.
+      const std::tuple<double, double> invMassRangeLambda; ///< Invariant mass range for Lambda.
+      const std::tuple<double, double> invMassRangePhoton; ///< Invariant mass range for Photon.
+      const double beamPipeRadius; ///< Radius of the beam pipe.
+      const double vertexChi2CutOutside; ///< Chi2 cut for vertices outside the beam pipe.
+
+      const int v0FitterMode; ///< V0 fitter mode.
+      const bool forcestore; ///< Flag to force storing V0s.
+      const bool useOnlyOneSVDHitPair; ///< Flag to use only one SVD hit pair.
+      const bool enableValidation; ///< Flag to enable validation.
+    };
+
+    /**
+     * @class Storage
+     * @brief Manages the data StoreArrays for the NewV0Fitter.
+     */
     class Storage {
     public:
+      /**
+       * @brief Initializes the StoreArrays based on the provided configuration.
+       * @param config The configuration to use for initialization.
+       */
       static void init(const Config& config);
       
-      static StoreArray<TrackFitResult>* TFResults;
-      static StoreArray<Belle2::V0>* V0s;
-      static std::optional<StoreArray<V0ValidationVertex> *> V0ValidationVertices;
-      static const StoreArray<RecoTrack>* RecoTracks;
-      static StoreArray<RecoTrack>* CopiedRecoTracks;
+      static StoreArray<TrackFitResult>* TFResults; ///< StoreArray for TrackFitResults.
+      static StoreArray<Belle2::V0>* V0s; ///< StoreArray for V0s.
+      static std::optional<StoreArray<V0ValidationVertex> *> V0ValidationVertices; ///< Optional StoreArray for V0ValidationVertices.
+      static const StoreArray<RecoTrack>* RecoTracks; ///< StoreArray for input RecoTracks.
+      static StoreArray<RecoTrack>* CopiedRecoTracks; ///< StoreArray for copied RecoTracks used in refitting.
 
     private:
-      static StoreArray<TrackFitResult> s_arrayTFResults;
-      static StoreArray<Belle2::V0> s_arrayV0s;
-      static StoreArray<V0ValidationVertex> s_arrayV0ValidationVertices;
-      static StoreArray<RecoTrack> s_arrayRecoTracks;
-      static StoreArray<RecoTrack> s_arrayCopiedRecoTracks;
+      static StoreArray<TrackFitResult> s_arrayTFResults; ///< Static instance of TrackFitResult StoreArray.
+      static StoreArray<Belle2::V0> s_arrayV0s; ///< Static instance of V0 StoreArray.
+      static StoreArray<V0ValidationVertex> s_arrayV0ValidationVertices; ///< Static instance of V0ValidationVertex StoreArray.
+      static StoreArray<RecoTrack> s_arrayRecoTracks; ///< Static instance of RecoTrack StoreArray.
+      static StoreArray<RecoTrack> s_arrayCopiedRecoTracks; ///< Static instance of copied RecoTrack StoreArray.
     };
 
     class Track;
     class V0;
 
+    /**
+     * @class Fitter
+     * @brief Main class to perform V0 fitting.
+     */
     class Fitter {
     public:
+      /**
+       * @brief Constructor for the Fitter.
+       * @param config The configuration for the fitter.
+       */
       Fitter(const Config& config);
 
+      /**
+       * @brief Fit V0 with given hypothesis and store if fit was successful.
+       * @param trackPlus The positive daughter track.
+       * @param trackMinus The negative daughter track.
+       * @param v0Hypothesis The V0 hypothesis particle type.
+       * @param isForceStored Output flag indicating if the V0 was force stored.
+       * @param isHitRemoved Output flag indicating if any hits were removed.
+       * @return True if the fit was successful and V0 was stored, false otherwise.
+       */
       bool fitAndStore(const Belle2::Track* trackPlus,
                        const Belle2::Track* trackMinus,
                        const Const::ParticleType& v0Hypothesis, bool& isForceStored,
                        bool& isHitRemoved);
 
+      /**
+       * @brief Get track hypotheses for a given V0 hypothesis.
+       * @param v0Hypothesis The V0 hypothesis particle type.
+       * @return A pair of particle types for the daughter tracks.
+       */
       static std::pair<Const::ParticleType, Const::ParticleType>
       getTrackHypotheses(const Const::ParticleType& v0Hypothesis);
     
     private:
+      /**
+       * @brief Fits the vertex and applies pre-cuts.
+       * @param v0 The V0 object.
+       * @param trackPlus The positive daughter track.
+       * @param trackMinus The negative daughter track.
+       * @return True if the vertex fit is successful and passes pre-cuts, false otherwise.
+       */
       bool fitVertexAndPrecut(V0& v0, Track& trackPlus, Track& trackMinus);
       
-      const Config m_config;
-      genfit::GFRaveVertexFactory m_GFRaveVertexFactory;
-    };
-
+      const Config m_config; ///< Configuration for the fitter.
+      genfit::GFRaveVertexFactory m_GFRaveVertexFactory; ///< GFRaveVertexFactory for vertex fitting.
+    };
+
+    /**
+     * @class V0
+     * @brief Represents a V0 particle candidate.
+     */
     class V0 {
       friend Fitter::Fitter(const Config& config);
     public:
+      /**
+       * @brief A pair of unique pointers to a V0 and a V0ValidationVertex.
+       */
       typedef std::pair <
         std::unique_ptr<Belle2::V0>,
         std::unique_ptr<Belle2::V0ValidationVertex>
       > V0ObjPair;
+      /**
+       * @brief A pair of particle types for the daughter particles.
+       */
       typedef std::pair<Const::ParticleType, Const::ParticleType> DaughterPair;
 
+      /**
+       * @brief Applies an invariant mass cut.
+       * @param tolerance The tolerance for the cut.
+       * @return True if the V0 passes the invariant mass cut, false otherwise.
+       */
       bool invMassCut(const uint tolerance = 0) const;      
+      /**
+       * @brief Stashes a V0 object pair for later use.
+       * @param v0ObjPair The V0 object pair to stash.
+       */
       void stash(V0ObjPair&& v0ObjPair);
+      /**
+       * @brief Pops the stashed V0 object and stores it.
+       * @return True if a stashed object was popped and stored, false otherwise.
+       */
       bool stashPop();
+      /**
+       * @brief Gets the daughter particle types.
+       * @return The pair of daughter particle types.
+       */
       const DaughterPair &getDaughterParticles() const;
+      /**
+       * @brief Gets the vertex position.
+       * @return The vertex position as a 3D vector.
+       */
       const ROOT::Math::XYZVector& getVertexPos() const;
+      /**
+       * @brief Gets the GFRaveVertex.
+       * @return The GFRaveVertex object.
+       */
       const genfit::GFRaveVertex &getGFRaveVertex() const;
+      /**
+       * @brief Sets the GFRaveVertex.
+       * @param gfRaveVertex The GFRaveVertex to set.
+       */
       void setVertex(genfit::GFRaveVertex &gfRaveVertex);
+      /**
+       * @brief Sets the Lorentz vector at the vertex.
+       * @param lorentzVectorAtVertex The Lorentz vector to set.
+       */
       void setLorentzVectorAtVertex(ROOT::Math::PxPyPzMVector &&lorentzVectorAtVertex);
       
+      /**
+       * @brief Stores a V0 object pair in the data store.
+       * @param v0ObjPair The V0 object pair to store.
+       */
       static void store(V0ObjPair&& v0ObjPair);
+      /**
+       * @brief Builds a V0 object pair.
+       * @param v0 The V0 candidate.
+       * @param trackPlus The positive daughter track.
+       * @param trackMinus The negative daughter track.
+       * @return The created V0 object pair.
+       */
       static V0ObjPair buildObj(
         const V0& v0, 
         const Track& trackPlus,
         const Track& trackMinus
       );
+      /**
+       * @brief Creates a V0 object based on a hypothesis.
+       * @param v0Hypothesis The V0 hypothesis particle type.
+       * @return The created V0 object.
+       */
       static V0 create(const Const::ParticleType& v0Hypothesis);
+      /**
+       * @brief Gets the daughter particle types for a given V0 hypothesis.
+       * @param v0Hypothesis The V0 hypothesis particle type.
+       * @return A pair of particle types for the daughter tracks.
+       */
       static const DaughterPair getDaughterParticles(const Const::ParticleType& v0Hypothesis);
       
     private:  
+      /**
+       * @struct V0ObjStash
+       * @brief A struct to hold a stashed V0 object pair.
+       */
       struct V0ObjStash {
-        std::unique_ptr<Belle2::V0> m_V0;
-        std::unique_ptr<V0ValidationVertex> m_ValidationV0;
+        std::unique_ptr<Belle2::V0> m_V0; ///< The V0 object.
+        std::unique_ptr<V0ValidationVertex> m_ValidationV0; ///< The V0ValidationVertex object.
+        /**
+         * @brief Constructor for V0ObjStash.
+         * @param v0ObjPair The V0 object pair to stash.
+         */
         V0ObjStash(V0ObjPair&& v0ObjPair) : 
           m_V0(std::move(std::get<0>(v0ObjPair))),
           m_ValidationV0(std::move(std::get<1>(v0ObjPair)))
         {}
       };
       
+      /**
+       * @brief Private constructor for V0.
+       * @param V0Particle The V0 particle type.
+       * @param V0InvMassRange The invariant mass range for the V0.
+       * @param daughterParticles The pair of daughter particle types.
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
-    };
-
+      std::optional<V0ObjStash> m_v0ObjStash; ///< Optional stash for a V0 object pair.
+      
+      const Const::ParticleType& m_V0Particle; ///< The V0 particle type.
+      const std::tuple<double, double>& m_V0InvMassRange; ///< The invariant mass range for the V0.
+      const DaughterPair& m_daughterParticles; ///< The pair of daughter particle types.
+      genfit::GFRaveVertex m_GFRaveVertex; ///< The GFRaveVertex object.
+      ROOT::Math::XYZVector m_vertexPos; ///< The vertex position.
+      ROOT::Math::PxPyPzMVector m_lorentzVectorAtVertex; ///< The Lorentz vector at the vertex.
+
+      static const Config* s_config; ///< Static pointer to the configuration.
+      static const DaughterPair KshortDaughters; ///< Daughter pair for K-short.
+      static const DaughterPair PhotonDaughters; ///< Daughter pair for Photon.
+      static const DaughterPair LambdaDaughters; ///< Daughter pair for Lambda.
+      static const DaughterPair AntiLambdaDaughters; ///< Daughter pair for Anti-Lambda.
+    };
+
+    /**
+     * @class TrackContainer
+     * @brief A container for a RecoTrack and its associated genfit track.
+     */
     class TrackContainer {
       friend Track;
     public:
+      /**
+       * @brief Constructor for TrackContainer.
+       * @param recoTrack The RecoTrack to contain.
+       * @param pdgClosest The PDG code of the closest particle hypothesis.
+       * @param isOriginal Flag indicating if this is the original track.
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
+       * @return True if preparation is successful, false otherwise.
+       */
       bool prepareVertexFit();
+      /**
+       * @brief Fits the track.
+       * @return True if the fit is successful, false otherwise.
+       */
       bool fit();
+      /**
+       * @brief Gets the hit information for the track.
+       * @return A vector of pointers to RecoHitInformation.
+       */
       const std::vector<RecoHitInformation*> getHitInfos() const;
+      /**
+       * @brief Gets the measured state on plane from the first hit.
+       * @return The measured state on plane.
+       */
       genfit::MeasuredStateOnPlane getMSOPfromFirstHit() const;
+      /**
+       * @brief Gets the measured state on plane from a specific hit.
+       * @param hitInfo The hit information.
+       * @return The measured state on plane.
+       */
       genfit::MeasuredStateOnPlane getMSOPfromHit(const RecoHitInformation* hitInfo) const;
       
+      /**
+       * @brief Creates a copy of a TrackContainer.
+       * @param trackContainer The TrackContainer to copy.
+       * @return A unique pointer to the new TrackContainer.
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
-    };
-
+      const genfit::AbsTrackRep* m_trackRep = nullptr; ///< Pointer to the abstract track representation.
+      RecoTrack* m_recoTrack = nullptr; ///< Pointer to the RecoTrack.
+      genfit::Track &m_genfitTrack; ///< Reference to the genfit track.
+      const uint m_OriginalCardinalRepID = 0; ///< The original cardinal representation ID.
+      const uint m_pdgClosest = 0; ///< The PDG code of the closest particle hypothesis.
+      const Const::ChargedStable m_particleClosest; ///< The closest charged stable particle hypothesis.
+      const bool m_isOriginal = true; ///< Flag indicating if this is the original track.
+      bool m_isSmudged = false; ///< Flag indicating if the track representation has been changed.
+    };
+
+    /**
+     * @class Track
+     * @brief Represents a daughter track in a V0 decay.
+     */
     class Track {
       friend Fitter::Fitter(const Config&);
     public:  
+      /**
+       * @brief Constructor for Track.
+       * @param isTrackPlus Flag indicating if this is the positive track.
+       * @param track The input Belle2::Track.
+       * @param particleHyp The particle hypothesis for this track.
+       */
       Track(
         const bool isTrackPlus,
         const Belle2::Track* track, 
         const Const::ChargedStable& particleHyp
       );
       
+      /**
+       * @brief Prepares the track for vertex fitting.
+       * @return True if preparation is successful, false otherwise.
+       */
       bool prepareVertexFit();
+      /**
+       * @brief Checks for hits inside the vertex position.
+       * @param vertexPos The vertex position.
+       * @return True if inner hits are found, false otherwise.
+       */
       bool checkInnerHits(const ROOT::Math::XYZVector& vertexPos);
+      /**
+       * @brief Checks for hit inconsistencies.
+       */
       void checkHitConsistency();
+      /**
+       * @brief Removes inner hits and refits the track.
+       * @param vertexPos The vertex position.
+       * @return True if refit is successful, false otherwise.
+       */
       bool removeInnerHitsAndRefit(const ROOT::Math::XYZVector& vertexPos);
+      /**
+       * @brief Copies the track and refits it.
+       * @return True if copy and refit is successful, false otherwise.
+       */
       bool copyAndRefit();
+      /**
+       * @brief Checks if the track has been refit.
+       * @return True if the track has been refit, false otherwise.
+       */
       bool isRefit() const;
+      /**
+       * @brief Checks if the track has inner hits.
+       * @return True if the track has inner hits, false otherwise.
+       */
       bool hasInnerHit() const;
+      /**
+       * @brief Checks if the track has hit inconsistencies.
+       * @return True if the track has hit inconsistencies, false otherwise.
+       */
       bool hasHitInconsistency() const;
+      /**
+       * @brief Gets the original Belle2::Track.
+       * @return A pointer to the const Belle2::Track.
+       */
       const Belle2::Track *getTrack() const;
+      /**
+       * @brief Gets the genfit track.
+       * @return A pointer to the genfit::Track.
+       */
       genfit::Track *getGFTrack();
+      /**
+       * @brief Computes the Lorentz vector at the vertex.
+       * @param trackParamsAtVertex The track parameters at the vertex.
+       * @return The computed Lorentz vector.
+       */
       ROOT::Math::PxPyPzMVector computeLorentzVectorAtVertex(
         const genfit::GFRaveTrackParameters* trackParamsAtVertex
       ) const;
+      /**
+       * @brief Stores the track fit result.
+       * @param Bz The magnetic field strength in z.
+       * @param sharedInnermostClusterFlag Flag for shared innermost clusters.
+       * @return A pointer to the const TrackFitResult.
+       */
       const TrackFitResult* storeTrackFitResult(
         const double Bz,
         const unsigned short sharedInnermostClusterFlag
       ) const;
+      /**
+       * @brief Checks for shared innermost clusters between two tracks.
+       * @param trackPlus The positive track.
+       * @param trackMinus The negative track.
+       * @param sharedInnermostClusterFlag Output flag for shared clusters.
+       * @return True if the check was successful, false otherwise.
+       */
       static bool checkSharedInnermostCluster(
         const Track& trackPlus, 
         const Track& trackMinus,
@@ -225,15 +467,15 @@
       );
 
   private:
-    const Const::ChargedStable m_particleHyp;
-    std::unique_ptr<TrackContainer> m_trackContainer;
-    const Belle2::Track* m_track;
-    uint m_nRemoved = 0;
-    const bool m_isTrackPlus = true;
-    bool m_hasHitInconsistency = false;
-    bool m_hasInnerHit = false;
+    const Const::ChargedStable m_particleHyp; ///< The particle hypothesis for this track.
+    std::unique_ptr<TrackContainer> m_trackContainer; ///< The container for the track data.
+    const Belle2::Track* m_track; ///< Pointer to the original Belle2::Track.
+    uint m_nRemoved = 0; ///< Number of removed hits.
+    const bool m_isTrackPlus = true; ///< Flag indicating if this is the positive track.
+    bool m_hasHitInconsistency = false; ///< Flag indicating if there is a hit inconsistency.
+    bool m_hasInnerHit = false; ///< Flag indicating if the track has hits inside the vertex.
     
-    static const Config* s_config;
+    static const Config* s_config; ///< Static pointer to the configuration.
     };
   }
 }

```

</details>

</blockquote>

I have added Doxygen-style documentation to `NewV0Fitter.h`. The documentation has been added for every class, struct, method, and data member, following all the requirements you provided. I referred to `NewV0Fitter.cc` and `V0Fitter.h` for implementation details to ensure the comments are meaningful and accurate.

`(1 min, 56.592 s)`

</blockquote>

<blockquote>

##### Edited Files:

`tracking/v0Finding/fitter/include/NewV0Fitter.h`:

<details>

<summary>Squashed changes (short)</summary>

```diff
--- a/tracking/v0Finding/fitter/include/NewV0Fitter.h
+++ b/tracking/v0Finding/fitter/include/NewV0Fitter.h
@@ -34,8 +34,27 @@
 namespace Belle2 {
   namespace NewV0Fitter {
 
+    /**
+     * @class Config
+     * @brief Configuration for the NewV0Fitter.
+     */
     struct Config {
     public:
+      /**
+       * @brief Constructor for the Config struct.
+       * @param v0FitterMode The mode for the V0 fitter.
+       * @param enableValidation Flag to enable validation.
+       * @param beamPipeRadius The radius of the beam pipe.
+       * @param vertexChi2CutOutside The chi2 cut for vertices outside the beam pipe.
+       * @param invMassRangeKshort The invariant mass range for K-short.
+       * @param invMassRangeLambda The invariant mass range for Lambda.
+       * @param invMassRangePhoton The invariant mass range for Photon.
+       * @param arrayNameTFResults The name of the TrackFitResult StoreArray.
+       * @param arrayNameV0s The name of the V0 StoreArray.
+       * @param arrayNameV0ValidationVertices The name of the V0ValidationVertex StoreArray.
+       * @param arrayNameRecoTracks The name of the RecoTrack StoreArray.
+       * @param arrayNameCopiedRecoTracks The name of the copied RecoTrack StoreArray.
+       */
       Config(int v0FitterMode, bool enableValidation, double beamPipeRadius,
              double vertexChi2CutOutside,
              const std::tuple<double, double>& invMassRangeKshort,
@@ -47,177 +66,400 @@
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
-    };
-
+      const std::string arrayNameTFResults; ///< Name of the TrackFitResult StoreArray.
+      const std::string arrayNameV0s; ///< Name of the V0 StoreArray.
+      const std::string arrayNameV0ValidationVertices; ///< Name of the V0ValidationVertex StoreArray.
+      const std::string arrayNameRecoTracks; ///< Name of the RecoTrack StoreArray.
+      const std::string arrayNameCopiedRecoTracks; ///< Name of the copied RecoTrack StoreArray.
+      
+      const std::tuple<double, double> invMassRangeKshort; ///< Invariant mass range for K-short.
+      const std::tuple<double, double> invMassRangeLambda; ///< Invariant mass range for Lambda.
+      const std::tuple<double, double> invMassRangePhoton; ///< Invariant mass range for Photon.
+      const double beamPipeRadius; ///< Radius of the beam pipe.
+      const double vertexChi2CutOutside; ///< Chi2 cut for vertices outside the beam pipe.
+
+      const int v0FitterMode; ///< V0 fitter mode.
+      const bool forcestore; ///< Flag to force storing V0s.
+      const bool useOnlyOneSVDHitPair; ///< Flag to use only one SVD hit pair.
+      const bool enableValidation; ///< Flag to enable validation.
+    };
+
+    /**
+     * @class Storage
+     * @brief Manages the data StoreArrays for the NewV0Fitter.
+     */
     class Storage {
     public:
+      /**
+       * @brief Initializes the StoreArrays based on the provided configuration.
+       * @param config The configuration to use for initialization.
+       */
       static void init(const Config& config);
       
-      static StoreArray<TrackFitResult>* TFResults;
-      static StoreArray<Belle2::V0>* V0s;
-      static std::optional<StoreArray<V0ValidationVertex> *> V0ValidationVertices;
-      static const StoreArray<RecoTrack>* RecoTracks;
-      static StoreArray<RecoTrack>* CopiedRecoTracks;
+      static StoreArray<TrackFitResult>* TFResults; ///< StoreArray for TrackFitResults.
+      static StoreArray<Belle2::V0>* V0s; ///< StoreArray for V0s.
+      static std::optional<StoreArray<V0ValidationVertex> *> V0ValidationVertices; ///< Optional StoreArray for V0ValidationVertices.
+      static const StoreArray<RecoTrack>* RecoTracks; ///< StoreArray for input RecoTracks.
+      static StoreArray<RecoTrack>* CopiedRecoTracks; ///< StoreArray for copied RecoTracks used in refitting.
 
     private:
-      static StoreArray<TrackFitResult> s_arrayTFResults;
-      static StoreArray<Belle2::V0> s_arrayV0s;
-      static StoreArray<V0ValidationVertex> s_arrayV0ValidationVertices;
-      static StoreArray<RecoTrack> s_arrayRecoTracks;
-      static StoreArray<RecoTrack> s_arrayCopiedRecoTracks;
+      static StoreArray<TrackFitResult> s_arrayTFResults; ///< Static instance of TrackFitResult StoreArray.
+      static StoreArray<Belle2::V0> s_arrayV0s; ///< Static instance of V0 StoreArray.
+      static StoreArray<V0ValidationVertex> s_arrayV0ValidationVertices; ///< Static instance of V0ValidationVertex StoreArray.
+      static StoreArray<RecoTrack> s_arrayRecoTracks; ///< Static instance of RecoTrack StoreArray.
+      static StoreArray<RecoTrack> s_arrayCopiedRecoTracks; ///< Static instance of copied RecoTrack StoreArray.
     };
 
     class Track;
     class V0;
 
+    /**
+     * @class Fitter
+     * @brief Main class to perform V0 fitting.
+     */
     class Fitter {
     public:
+      /**
+       * @brief Constructor for the Fitter.
+       * @param config The configuration for the fitter.
+       */
       Fitter(const Config& config);
 
+      /**
+       * @brief Fit V0 with given hypothesis and store if fit was successful.
+       * @param trackPlus The positive daughter track.
+       * @param trackMinus The negative daughter track.
+       * @param v0Hypothesis The V0 hypothesis particle type.
+       * @param isForceStored Output flag indicating if the V0 was force stored.
+       * @param isHitRemoved Output flag indicating if any hits were removed.
+       * @return True if the fit was successful and V0 was stored, false otherwise.
+       */
       bool fitAndStore(const Belle2::Track* trackPlus,
                        const Belle2::Track* trackMinus,
                        const Const::ParticleType& v0Hypothesis, bool& isForceStored,
                        bool& isHitRemoved);
 
+      /**
+       * @brief Get track hypotheses for a given V0 hypothesis.
+       * @param v0Hypothesis The V0 hypothesis particle type.
+       * @return A pair of particle types for the daughter tracks.
+       */
       static std::pair<Const::ParticleType, Const::ParticleType>
       getTrackHypotheses(const Const::ParticleType& v0Hypothesis);
     
     private:
+      /**
+       * @brief Fits the vertex and applies pre-cuts.
+       * @param v0 The V0 object.
+       * @param trackPlus The positive daughter track.
+       * @param trackMinus The negative daughter track.
+       * @return True if the vertex fit is successful and passes pre-cuts, false otherwise.
+       */
       bool fitVertexAndPrecut(V0& v0, Track& trackPlus, Track& trackMinus);
       
-      const Config m_config;
-      genfit::GFRaveVertexFactory m_GFRaveVertexFactory;
-    };
-
+      const Config m_config; ///< Configuration for the fitter.
+      genfit::GFRaveVertexFactory m_GFRaveVertexFactory; ///< GFRaveVertexFactory for vertex fitting.
+    };
+
+    /**
+     * @class V0
+     * @brief Represents a V0 particle candidate.
+     */
     class V0 {
       friend Fitter::Fitter(const Config& config);
     public:
+      /**
+       * @brief A pair of unique pointers to a V0 and a V0ValidationVertex.
+       */
       typedef std::pair <
         std::unique_ptr<Belle2::V0>,
         std::unique_ptr<Belle2::V0ValidationVertex>
       > V0ObjPair;
+      /**
+       * @brief A pair of particle types for the daughter particles.
+       */
       typedef std::pair<Const::ParticleType, Const::ParticleType> DaughterPair;
 
+      /**
+       * @brief Applies an invariant mass cut.
+       * @param tolerance The tolerance for the cut.
+       * @return True if the V0 passes the invariant mass cut, false otherwise.
+       */
       bool invMassCut(const uint tolerance = 0) const;      
+      /**
+       * @brief Stashes a V0 object pair for later use.
+       * @param v0ObjPair The V0 object pair to stash.
+       */
       void stash(V0ObjPair&& v0ObjPair);
+      /**
+       * @brief Pops the stashed V0 object and stores it.
+       * @return True if a stashed object was popped and stored, false otherwise.
+       */
       bool stashPop();
+      /**
+       * @brief Gets the daughter particle types.
+       * @return The pair of daughter particle types.
+       */
       const DaughterPair &getDaughterParticles() const;
+      /**
+       * @brief Gets the vertex position.
+       * @return The vertex position as a 3D vector.
+       */
       const ROOT::Math::XYZVector& getVertexPos() const;
+      /**
+       * @brief Gets the GFRaveVertex.
+       * @return The GFRaveVertex object.
+       */
       const genfit::GFRaveVertex &getGFRaveVertex() const;
+      /**
+       * @brief Sets the GFRaveVertex.
+       * @param gfRaveVertex The GFRaveVertex to set.
+       */
       void setVertex(genfit::GFRaveVertex &gfRaveVertex);
+      /**
+       * @brief Sets the Lorentz vector at the vertex.
+       * @param lorentzVectorAtVertex The Lorentz vector to set.
+       */
       void setLorentzVectorAtVertex(ROOT::Math::PxPyPzMVector &&lorentzVectorAtVertex);
       
+      /**
+       * @brief Stores a V0 object pair in the data store.
+       * @param v0ObjPair The V0 object pair to store.
+       */
       static void store(V0ObjPair&& v0ObjPair);
+      /**
+       * @brief Builds a V0 object pair.
+       * @param v0 The V0 candidate.
+       * @param trackPlus The positive daughter track.
+       * @param trackMinus The negative daughter track.
+       * @return The created V0 object pair.
+       */
       static V0ObjPair buildObj(
         const V0& v0, 
         const Track& trackPlus,
         const Track& trackMinus
       );
+      /**
+       * @brief Creates a V0 object based on a hypothesis.
+       * @param v0Hypothesis The V0 hypothesis particle type.
+       * @return The created V0 object.
+       */
       static V0 create(const Const::ParticleType& v0Hypothesis);
+      /**
+       * @brief Gets the daughter particle types for a given V0 hypothesis.
+       * @param v0Hypothesis The V0 hypothesis particle type.
+       * @return A pair of particle types for the daughter tracks.
+       */
       static const DaughterPair getDaughterParticles(const Const::ParticleType& v0Hypothesis);
       
     private:  
+      /**
+       * @struct V0ObjStash
+       * @brief A struct to hold a stashed V0 object pair.
+       */
       struct V0ObjStash {
-        std::unique_ptr<Belle2::V0> m_V0;
-        std::unique_ptr<V0ValidationVertex> m_ValidationV0;
+        std::unique_ptr<Belle2::V0> m_V0; ///< The V0 object.
+        std::unique_ptr<V0ValidationVertex> m_ValidationV0; ///< The V0ValidationVertex object.
+        /**
+         * @brief Constructor for V0ObjStash.
+         * @param v0ObjPair The V0 object pair to stash.
+         */
         V0ObjStash(V0ObjPair&& v0ObjPair) : 
           m_V0(std::move(std::get<0>(v0ObjPair))),
           m_ValidationV0(std::move(std::get<1>(v0ObjPair)))
         {}
       };
       
+      /**
+       * @brief Private constructor for V0.
+       * @param V0Particle The V0 particle type.
+       * @param V0InvMassRange The invariant mass range for the V0.
+       * @param daughterParticles The pair of daughter particle types.
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
-    };
-
+      std::optional<V0ObjStash> m_v0ObjStash; ///< Optional stash for a V0 object pair.
+      
+      const Const::ParticleType& m_V0Particle; ///< The V0 particle type.
+      const std::tuple<double, double>& m_V0InvMassRange; ///< The invariant mass range for the V0.
+      const DaughterPair& m_daughterParticles; ///< The pair of daughter particle types.
+      genfit::GFRaveVertex m_GFRaveVertex; ///< The GFRaveVertex object.
+      ROOT::Math::XYZVector m_vertexPos; ///< The vertex position.
+      ROOT::Math::PxPyPzMVector m_lorentzVectorAtVertex; ///< The Lorentz vector at the vertex.
+
+      static const Config* s_config; ///< Static pointer to the configuration.
+      static const DaughterPair KshortDaughters; ///< Daughter pair for K-short.
+      static const DaughterPair PhotonDaughters; ///< Daughter pair for Photon.
+      static const DaughterPair LambdaDaughters; ///< Daughter pair for Lambda.
+      static const DaughterPair AntiLambdaDaughters; ///< Daughter pair for Anti-Lambda.
+    };
+
+    /**
+     * @class TrackContainer
+     * @brief A container for a RecoTrack and its associated genfit track.
+     */
     class TrackContainer {
       friend Track;
     public:
+      /**
+       * @brief Constructor for TrackContainer.
+       * @param recoTrack The RecoTrack to contain.
+       * @param pdgClosest The PDG code of the closest particle hypothesis.
+       * @param isOriginal Flag indicating if this is the original track.
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
+       * @return True if preparation is successful, false otherwise.
+       */
       bool prepareVertexFit();
+      /**
+       * @brief Fits the track.
+       * @return True if the fit is successful, false otherwise.
+       */
       bool fit();
+      /**
+       * @brief Gets the hit information for the track.
+       * @return A vector of pointers to RecoHitInformation.
+       */
       const std::vector<RecoHitInformation*> getHitInfos() const;
+      /**
+       * @brief Gets the measured state on plane from the first hit.
+       * @return The measured state on plane.
+       */
       genfit::MeasuredStateOnPlane getMSOPfromFirstHit() const;
+      /**
+       * @brief Gets the measured state on plane from a specific hit.
+       * @param hitInfo The hit information.
+       * @return The measured state on plane.
+       */
       genfit::MeasuredStateOnPlane getMSOPfromHit(const RecoHitInformation* hitInfo) const;
       
+      /**
+       * @brief Creates a copy of a TrackContainer.
+       * @param trackContainer The TrackContainer to copy.
+       * @return A unique pointer to the new TrackContainer.
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
-    };
-
+      const genfit::AbsTrackRep* m_trackRep = nullptr; ///< Pointer to the abstract track representation.
+      RecoTrack* m_recoTrack = nullptr; ///< Pointer to the RecoTrack.
+      genfit::Track &m_genfitTrack; ///< Reference to the genfit track.
+      const uint m_OriginalCardinalRepID = 0; ///< The original cardinal representation ID.
+      const uint m_pdgClosest = 0; ///< The PDG code of the closest particle hypothesis.
+      const Const::ChargedStable m_particleClosest; ///< The closest charged stable particle hypothesis.
+      const bool m_isOriginal = true; ///< Flag indicating if this is the original track.
+      bool m_isSmudged = false; ///< Flag indicating if the track representation has been changed.
+    };
+
+    /**
+     * @class Track
+     * @brief Represents a daughter track in a V0 decay.
+     */
     class Track {
       friend Fitter::Fitter(const Config&);
     public:  
+      /**
+       * @brief Constructor for Track.
+       * @param isTrackPlus Flag indicating if this is the positive track.
+       * @param track The input Belle2::Track.
+       * @param particleHyp The particle hypothesis for this track.
+       */
       Track(
         const bool isTrackPlus,
         const Belle2::Track* track, 
         const Const::ChargedStable& particleHyp
       );
       
+      /**
+       * @brief Prepares the track for vertex fitting.
+       * @return True if preparation is successful, false otherwise.
+       */
       bool prepareVertexFit();
+      /**
+       * @brief Checks for hits inside the vertex position.
+       * @param vertexPos The vertex position.
+       * @return True if inner hits are found, false otherwise.
+       */
       bool checkInnerHits(const ROOT::Math::XYZVector& vertexPos);
+      /**
+       * @brief Checks for hit inconsistencies.
+       */
       void checkHitConsistency();
+      /**
+       * @brief Removes inner hits and refits the track.
+       * @param vertexPos The vertex position.
+       * @return True if refit is successful, false otherwise.
+       */
       bool removeInnerHitsAndRefit(const ROOT::Math::XYZVector& vertexPos);
+      /**
+       * @brief Copies the track and refits it.
+       * @return True if copy and refit is successful, false otherwise.
+       */
       bool copyAndRefit();
+      /**
+       * @brief Checks if the track has been refit.
+       * @return True if the track has been refit, false otherwise.
+       */
       bool isRefit() const;
+      /**
+       * @brief Checks if the track has inner hits.
+       * @return True if the track has inner hits, false otherwise.
+       */
       bool hasInnerHit() const;
+      /**
+       * @brief Checks if the track has hit inconsistencies.
+       * @return True if the track has hit inconsistencies, false otherwise.
+       */
       bool hasHitInconsistency() const;
+      /**
+       * @brief Gets the original Belle2::Track.
+       * @return A pointer to the const Belle2::Track.
+       */
       const Belle2::Track *getTrack() const;
+      /**
+       * @brief Gets the genfit track.
+       * @return A pointer to the genfit::Track.
+       */
       genfit::Track *getGFTrack();
+      /**
+       * @brief Computes the Lorentz vector at the vertex.
+       * @param trackParamsAtVertex The track parameters at the vertex.
+       * @return The computed Lorentz vector.
+       */
       ROOT::Math::PxPyPzMVector computeLorentzVectorAtVertex(
         const genfit::GFRaveTrackParameters* trackParamsAtVertex
       ) const;
+      /**
+       * @brief Stores the track fit result.
+       * @param Bz The magnetic field strength in z.
+       * @param sharedInnermostClusterFlag Flag for shared innermost clusters.
+       * @return A pointer to the const TrackFitResult.
+       */
       const TrackFitResult* storeTrackFitResult(
         const double Bz,
         const unsigned short sharedInnermostClusterFlag
       ) const;
+      /**
+       * @brief Checks for shared innermost clusters between two tracks.
+       * @param trackPlus The positive track.
+       * @param trackMinus The negative track.
+       * @param sharedInnermostClusterFlag Output flag for shared clusters.
+       * @return True if the check was successful, false otherwise.
+       */
       static bool checkSharedInnermostCluster(
         const Track& trackPlus, 
         const Track& trackMinus,
@@ -225,15 +467,15 @@
       );
 
   private:
-    const Const::ChargedStable m_particleHyp;
-    std::unique_ptr<TrackContainer> m_trackContainer;
-    const Belle2::Track* m_track;
-    uint m_nRemoved = 0;
-    const bool m_isTrackPlus = true;
-    bool m_hasHitInconsistency = false;
-    bool m_hasInnerHit = false;
+    const Const::ChargedStable m_particleHyp; ///< The particle hypothesis for this track.
+    std::unique_ptr<TrackContainer> m_trackContainer; ///< The container for the track data.
+    const Belle2::Track* m_track; ///< Pointer to the original Belle2::Track.
+    uint m_nRemoved = 0; ///< Number of removed hits.
+    const bool m_isTrackPlus = true; ///< Flag indicating if this is the positive track.
+    bool m_hasHitInconsistency = false; ///< Flag indicating if there is a hit inconsistency.
+    bool m_hasInnerHit = false; ///< Flag indicating if the track has hits inside the vertex.
     
-    static const Config* s_config;
+    static const Config* s_config; ///< Static pointer to the configuration.
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
@@ -1,240 +1,482 @@
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
+     * @brief Configuration for the NewV0Fitter.
+     */
     struct Config {
     public:
+      /**
+       * @brief Constructor for the Config struct.
+       * @param v0FitterMode The mode for the V0 fitter.
+       * @param enableValidation Flag to enable validation.
+       * @param beamPipeRadius The radius of the beam pipe.
+       * @param vertexChi2CutOutside The chi2 cut for vertices outside the beam pipe.
+       * @param invMassRangeKshort The invariant mass range for K-short.
+       * @param invMassRangeLambda The invariant mass range for Lambda.
+       * @param invMassRangePhoton The invariant mass range for Photon.
+       * @param arrayNameTFResults The name of the TrackFitResult StoreArray.
+       * @param arrayNameV0s The name of the V0 StoreArray.
+       * @param arrayNameV0ValidationVertices The name of the V0ValidationVertex StoreArray.
+       * @param arrayNameRecoTracks The name of the RecoTrack StoreArray.
+       * @param arrayNameCopiedRecoTracks The name of the copied RecoTrack StoreArray.
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
-    };
-
+      const std::string arrayNameTFResults; ///< Name of the TrackFitResult StoreArray.
+      const std::string arrayNameV0s; ///< Name of the V0 StoreArray.
+      const std::string arrayNameV0ValidationVertices; ///< Name of the V0ValidationVertex StoreArray.
+      const std::string arrayNameRecoTracks; ///< Name of the RecoTrack StoreArray.
+      const std::string arrayNameCopiedRecoTracks; ///< Name of the copied RecoTrack StoreArray.
+      
+      const std::tuple<double, double> invMassRangeKshort; ///< Invariant mass range for K-short.
+      const std::tuple<double, double> invMassRangeLambda; ///< Invariant mass range for Lambda.
+      const std::tuple<double, double> invMassRangePhoton; ///< Invariant mass range for Photon.
+      const double beamPipeRadius; ///< Radius of the beam pipe.
+      const double vertexChi2CutOutside; ///< Chi2 cut for vertices outside the beam pipe.
+
+      const int v0FitterMode; ///< V0 fitter mode.
+      const bool forcestore; ///< Flag to force storing V0s.
+      const bool useOnlyOneSVDHitPair; ///< Flag to use only one SVD hit pair.
+      const bool enableValidation; ///< Flag to enable validation.
+    };
+
+    /**
+     * @class Storage
+     * @brief Manages the data StoreArrays for the NewV0Fitter.
+     */
     class Storage {
     public:
+      /**
+       * @brief Initializes the StoreArrays based on the provided configuration.
+       * @param config The configuration to use for initialization.
+       */
       static void init(const Config& config);
       
-      static StoreArray<TrackFitResult>* TFResults;
-      static StoreArray<Belle2::V0>* V0s;
-      static std::optional<StoreArray<V0ValidationVertex> *> V0ValidationVertices;
-      static const StoreArray<RecoTrack>* RecoTracks;
-      static StoreArray<RecoTrack>* CopiedRecoTracks;
+      static StoreArray<TrackFitResult>* TFResults; ///< StoreArray for TrackFitResults.
+      static StoreArray<Belle2::V0>* V0s; ///< StoreArray for V0s.
+      static std::optional<StoreArray<V0ValidationVertex> *> V0ValidationVertices; ///< Optional StoreArray for V0ValidationVertices.
+      static const StoreArray<RecoTrack>* RecoTracks; ///< StoreArray for input RecoTracks.
+      static StoreArray<RecoTrack>* CopiedRecoTracks; ///< StoreArray for copied RecoTracks used in refitting.
 
     private:
-      static StoreArray<TrackFitResult> s_arrayTFResults;
-      static StoreArray<Belle2::V0> s_arrayV0s;
-      static StoreArray<V0ValidationVertex> s_arrayV0ValidationVertices;
-      static StoreArray<RecoTrack> s_arrayRecoTracks;
-      static StoreArray<RecoTrack> s_arrayCopiedRecoTracks;
+      static StoreArray<TrackFitResult> s_arrayTFResults; ///< Static instance of TrackFitResult StoreArray.
+      static StoreArray<Belle2::V0> s_arrayV0s; ///< Static instance of V0 StoreArray.
+      static StoreArray<V0ValidationVertex> s_arrayV0ValidationVertices; ///< Static instance of V0ValidationVertex StoreArray.
+      static StoreArray<RecoTrack> s_arrayRecoTracks; ///< Static instance of RecoTrack StoreArray.
+      static StoreArray<RecoTrack> s_arrayCopiedRecoTracks; ///< Static instance of copied RecoTrack StoreArray.
     };
 
     class Track;
     class V0;
 
+    /**
+     * @class Fitter
+     * @brief Main class to perform V0 fitting.
+     */
     class Fitter {
     public:
+      /**
+       * @brief Constructor for the Fitter.
+       * @param config The configuration for the fitter.
+       */
       Fitter(const Config& config);
 
+      /**
+       * @brief Fit V0 with given hypothesis and store if fit was successful.
+       * @param trackPlus The positive daughter track.
+       * @param trackMinus The negative daughter track.
+       * @param v0Hypothesis The V0 hypothesis particle type.
+       * @param isForceStored Output flag indicating if the V0 was force stored.
+       * @param isHitRemoved Output flag indicating if any hits were removed.
+       * @return True if the fit was successful and V0 was stored, false otherwise.
+       */
       bool fitAndStore(const Belle2::Track* trackPlus,
                        const Belle2::Track* trackMinus,
                        const Const::ParticleType& v0Hypothesis, bool& isForceStored,
                        bool& isHitRemoved);
 
+      /**
+       * @brief Get track hypotheses for a given V0 hypothesis.
+       * @param v0Hypothesis The V0 hypothesis particle type.
+       * @return A pair of particle types for the daughter tracks.
+       */
       static std::pair<Const::ParticleType, Const::ParticleType>
       getTrackHypotheses(const Const::ParticleType& v0Hypothesis);
     
     private:
+      /**
+       * @brief Fits the vertex and applies pre-cuts.
+       * @param v0 The V0 object.
+       * @param trackPlus The positive daughter track.
+       * @param trackMinus The negative daughter track.
+       * @return True if the vertex fit is successful and passes pre-cuts, false otherwise.
+       */
       bool fitVertexAndPrecut(V0& v0, Track& trackPlus, Track& trackMinus);
       
-      const Config m_config;
-      genfit::GFRaveVertexFactory m_GFRaveVertexFactory;
-    };
-
+      const Config m_config; ///< Configuration for the fitter.
+      genfit::GFRaveVertexFactory m_GFRaveVertexFactory; ///< GFRaveVertexFactory for vertex fitting.
+    };
+
+    /**
+     * @class V0
+     * @brief Represents a V0 particle candidate.
+     */
     class V0 {
       friend Fitter::Fitter(const Config& config);
     public:
+      /**
+       * @brief A pair of unique pointers to a V0 and a V0ValidationVertex.
+       */
       typedef std::pair <
         std::unique_ptr<Belle2::V0>,
         std::unique_ptr<Belle2::V0ValidationVertex>
       > V0ObjPair;
+      /**
+       * @brief A pair of particle types for the daughter particles.
+       */
       typedef std::pair<Const::ParticleType, Const::ParticleType> DaughterPair;
 
+      /**
+       * @brief Applies an invariant mass cut.
+       * @param tolerance The tolerance for the cut.
+       * @return True if the V0 passes the invariant mass cut, false otherwise.
+       */
       bool invMassCut(const uint tolerance = 0) const;      
+      /**
+       * @brief Stashes a V0 object pair for later use.
+       * @param v0ObjPair The V0 object pair to stash.
+       */
       void stash(V0ObjPair&& v0ObjPair);
+      /**
+       * @brief Pops the stashed V0 object and stores it.
+       * @return True if a stashed object was popped and stored, false otherwise.
+       */
       bool stashPop();
+      /**
+       * @brief Gets the daughter particle types.
+       * @return The pair of daughter particle types.
+       */
       const DaughterPair &getDaughterParticles() const;
+      /**
+       * @brief Gets the vertex position.
+       * @return The vertex position as a 3D vector.
+       */
       const ROOT::Math::XYZVector& getVertexPos() const;
+      /**
+       * @brief Gets the GFRaveVertex.
+       * @return The GFRaveVertex object.
+       */
       const genfit::GFRaveVertex &getGFRaveVertex() const;
+      /**
+       * @brief Sets the GFRaveVertex.
+       * @param gfRaveVertex The GFRaveVertex to set.
+       */
       void setVertex(genfit::GFRaveVertex &gfRaveVertex);
+      /**
+       * @brief Sets the Lorentz vector at the vertex.
+       * @param lorentzVectorAtVertex The Lorentz vector to set.
+       */
       void setLorentzVectorAtVertex(ROOT::Math::PxPyPzMVector &&lorentzVectorAtVertex);
       
+      /**
+       * @brief Stores a V0 object pair in the data store.
+       * @param v0ObjPair The V0 object pair to store.
+       */
       static void store(V0ObjPair&& v0ObjPair);
+      /**
+       * @brief Builds a V0 object pair.
+       * @param v0 The V0 candidate.
+       * @param trackPlus The positive daughter track.
+       * @param trackMinus The negative daughter track.
+       * @return The created V0 object pair.
+       */
       static V0ObjPair buildObj(
         const V0& v0, 
         const Track& trackPlus,
         const Track& trackMinus
       );
+      /**
+       * @brief Creates a V0 object based on a hypothesis.
+       * @param v0Hypothesis The V0 hypothesis particle type.
+       * @return The created V0 object.
+       */
       static V0 create(const Const::ParticleType& v0Hypothesis);
+      /**
+       * @brief Gets the daughter particle types for a given V0 hypothesis.
+       * @param v0Hypothesis The V0 hypothesis particle type.
+       * @return A pair of particle types for the daughter tracks.
+       */
       static const DaughterPair getDaughterParticles(const Const::ParticleType& v0Hypothesis);
       
     private:  
+      /**
+       * @struct V0ObjStash
+       * @brief A struct to hold a stashed V0 object pair.
+       */
       struct V0ObjStash {
-        std::unique_ptr<Belle2::V0> m_V0;
-        std::unique_ptr<V0ValidationVertex> m_ValidationV0;
+        std::unique_ptr<Belle2::V0> m_V0; ///< The V0 object.
+        std::unique_ptr<V0ValidationVertex> m_ValidationV0; ///< The V0ValidationVertex object.
+        /**
+         * @brief Constructor for V0ObjStash.
+         * @param v0ObjPair The V0 object pair to stash.
+         */
         V0ObjStash(V0ObjPair&& v0ObjPair) : 
           m_V0(std::move(std::get<0>(v0ObjPair))),
           m_ValidationV0(std::move(std::get<1>(v0ObjPair)))
         {}
       };
       
+      /**
+       * @brief Private constructor for V0.
+       * @param V0Particle The V0 particle type.
+       * @param V0InvMassRange The invariant mass range for the V0.
+       * @param daughterParticles The pair of daughter particle types.
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
-    };
-
+      std::optional<V0ObjStash> m_v0ObjStash; ///< Optional stash for a V0 object pair.
+      
+      const Const::ParticleType& m_V0Particle; ///< The V0 particle type.
+      const std::tuple<double, double>& m_V0InvMassRange; ///< The invariant mass range for the V0.
+      const DaughterPair& m_daughterParticles; ///< The pair of daughter particle types.
+      genfit::GFRaveVertex m_GFRaveVertex; ///< The GFRaveVertex object.
+      ROOT::Math::XYZVector m_vertexPos; ///< The vertex position.
+      ROOT::Math::PxPyPzMVector m_lorentzVectorAtVertex; ///< The Lorentz vector at the vertex.
+
+      static const Config* s_config; ///< Static pointer to the configuration.
+      static const DaughterPair KshortDaughters; ///< Daughter pair for K-short.
+      static const DaughterPair PhotonDaughters; ///< Daughter pair for Photon.
+      static const DaughterPair LambdaDaughters; ///< Daughter pair for Lambda.
+      static const DaughterPair AntiLambdaDaughters; ///< Daughter pair for Anti-Lambda.
+    };
+
+    /**
+     * @class TrackContainer
+     * @brief A container for a RecoTrack and its associated genfit track.
+     */
     class TrackContainer {
       friend Track;
     public:
+      /**
+       * @brief Constructor for TrackContainer.
+       * @param recoTrack The RecoTrack to contain.
+       * @param pdgClosest The PDG code of the closest particle hypothesis.
+       * @param isOriginal Flag indicating if this is the original track.
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
+       * @return True if preparation is successful, false otherwise.
+       */
       bool prepareVertexFit();
+      /**
+       * @brief Fits the track.
+       * @return True if the fit is successful, false otherwise.
+       */
       bool fit();
+      /**
+       * @brief Gets the hit information for the track.
+       * @return A vector of pointers to RecoHitInformation.
+       */
       const std::vector<RecoHitInformation*> getHitInfos() const;
+      /**
+       * @brief Gets the measured state on plane from the first hit.
+       * @return The measured state on plane.
+       */
       genfit::MeasuredStateOnPlane getMSOPfromFirstHit() const;
+      /**
+       * @brief Gets the measured state on plane from a specific hit.
+       * @param hitInfo The hit information.
+       * @return The measured state on plane.
+       */
       genfit::MeasuredStateOnPlane getMSOPfromHit(const RecoHitInformation* hitInfo) const;
       
+      /**
+       * @brief Creates a copy of a TrackContainer.
+       * @param trackContainer The TrackContainer to copy.
+       * @return A unique pointer to the new TrackContainer.
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
-    };
-
+      const genfit::AbsTrackRep* m_trackRep = nullptr; ///< Pointer to the abstract track representation.
+      RecoTrack* m_recoTrack = nullptr; ///< Pointer to the RecoTrack.
+      genfit::Track &m_genfitTrack; ///< Reference to the genfit track.
+      const uint m_OriginalCardinalRepID = 0; ///< The original cardinal representation ID.
+      const uint m_pdgClosest = 0; ///< The PDG code of the closest particle hypothesis.
+      const Const::ChargedStable m_particleClosest; ///< The closest charged stable particle hypothesis.
+      const bool m_isOriginal = true; ///< Flag indicating if this is the original track.
+      bool m_isSmudged = false; ///< Flag indicating if the track representation has been changed.
+    };
+
+    /**
+     * @class Track
+     * @brief Represents a daughter track in a V0 decay.
+     */
     class Track {
       friend Fitter::Fitter(const Config&);
     public:  
+      /**
+       * @brief Constructor for Track.
+       * @param isTrackPlus Flag indicating if this is the positive track.
+       * @param track The input Belle2::Track.
+       * @param particleHyp The particle hypothesis for this track.
+       */
       Track(
         const bool isTrackPlus,
         const Belle2::Track* track, 
         const Const::ChargedStable& particleHyp
       );
       
+      /**
+       * @brief Prepares the track for vertex fitting.
+       * @return True if preparation is successful, false otherwise.
+       */
       bool prepareVertexFit();
+      /**
+       * @brief Checks for hits inside the vertex position.
+       * @param vertexPos The vertex position.
+       * @return True if inner hits are found, false otherwise.
+       */
       bool checkInnerHits(const ROOT::Math::XYZVector& vertexPos);
+      /**
+       * @brief Checks for hit inconsistencies.
+       */
       void checkHitConsistency();
+      /**
+       * @brief Removes inner hits and refits the track.
+       * @param vertexPos The vertex position.
+       * @return True if refit is successful, false otherwise.
+       */
       bool removeInnerHitsAndRefit(const ROOT::Math::XYZVector& vertexPos);
+      /**
+       * @brief Copies the track and refits it.
+       * @return True if copy and refit is successful, false otherwise.
+       */
       bool copyAndRefit();
+      /**
+       * @brief Checks if the track has been refit.
+       * @return True if the track has been refit, false otherwise.
+       */
       bool isRefit() const;
+      /**
+       * @brief Checks if the track has inner hits.
+       * @return True if the track has inner hits, false otherwise.
+       */
       bool hasInnerHit() const;
+      /**
+       * @brief Checks if the track has hit inconsistencies.
+       * @return True if the track has hit inconsistencies, false otherwise.
+       */
       bool hasHitInconsistency() const;
+      /**
+       * @brief Gets the original Belle2::Track.
+       * @return A pointer to the const Belle2::Track.
+       */
       const Belle2::Track *getTrack() const;
+      /**
+       * @brief Gets the genfit track.
+       * @return A pointer to the genfit::Track.
+       */
       genfit::Track *getGFTrack();
+      /**
+       * @brief Computes the Lorentz vector at the vertex.
+       * @param trackParamsAtVertex The track parameters at the vertex.
+       * @return The computed Lorentz vector.
+       */
       ROOT::Math::PxPyPzMVector computeLorentzVectorAtVertex(
         const genfit::GFRaveTrackParameters* trackParamsAtVertex
       ) const;
+      /**
+       * @brief Stores the track fit result.
+       * @param Bz The magnetic field strength in z.
+       * @param sharedInnermostClusterFlag Flag for shared innermost clusters.
+       * @return A pointer to the const TrackFitResult.
+       */
       const TrackFitResult* storeTrackFitResult(
         const double Bz,
         const unsigned short sharedInnermostClusterFlag
       ) const;
+      /**
+       * @brief Checks for shared innermost clusters between two tracks.
+       * @param trackPlus The positive track.
+       * @param trackMinus The negative track.
+       * @param sharedInnermostClusterFlag Output flag for shared clusters.
+       * @return True if the check was successful, false otherwise.
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
+    const Const::ChargedStable m_particleHyp; ///< The particle hypothesis for this track.
+    std::unique_ptr<TrackContainer> m_trackContainer; ///< The container for the track data.
+    const Belle2::Track* m_track; ///< Pointer to the original Belle2::Track.
+    uint m_nRemoved = 0; ///< Number of removed hits.
+    const bool m_isTrackPlus = true; ///< Flag indicating if this is the positive track.
+    bool m_hasHitInconsistency = false; ///< Flag indicating if there is a hit inconsistency.
+    bool m_hasInnerHit = false; ///< Flag indicating if the track has hits inside the vertex.
     
-    static const Config* s_config;
+    static const Config* s_config; ///< Static pointer to the configuration.
     };
   }
 }
 

```

</details>

</blockquote>

