You are a Principal Audio Software Engineer and Systems Architect specializing in C++ audio DSP, Music Information Retrieval (MIR), and local client-server web architectures.

I want to build a local, self-hosted web application that recursively scans my music library (`/home/Music/<genre>/<artist>/<album>/<track>`), extracts audio features, and enables dynamic M3U playlist generation based on stats (e.g., top 10 songs by BPM, danceability, energy).

My initial assumptions and goals:
1. Core engine in C++ for performance.
2. Feature extraction (BPM, danceability, key, energy) using Essentia or similar C++ toolkits.
3. Audio fingerprinting / identification behavior comparable to AcoustID / Chromaprint.
4. Interactive recursive scanning per genre folder with user confirmation (proceed/skip).
5. Dynamic export of M3U playlists based on multi-attribute queries.

### Your Direct Instructions:
DO NOT write implementation code immediately. First, execute the following two phases:

---

### Phase 1: The Technical Interrogation & Gap Analysis
Grill me directly on the engineering, algorithmic, and architectural trade-offs inherent in this specification. Specifically challenge and question me on:

1. **Feature Extraction vs. AcoustID Fingerprinting:**
   - Address the fundamental distinction between exact audio fingerprinting (AcoustID/Chromaprint for hash-based identification against MusicBrainz) versus semantic MIR feature analysis (Essentia extraction for BPM, key, danceability).
   - Clarify if we are using standard Essentia standard mode vs. streaming mode, or pre-trained ML models (TensorFlow/ONNX runtime bindings via Essentia) for subjective metrics like danceability.

2. **C++ Web Architecture & IPC:**
   - Challenge how the C++ core interfaces with the web tier:
     - Standalone monolithic C++ HTTP daemon (e.g., Crow, oat++, httplib, Pistache)?
     - C++ CLI daemon exposing an IPC/RPC layer (gRPC, Unix domain sockets, SQLite) consumed by a lightweight backend (Go, Python/FastAPI, Node.js)?
     - Native C++ engine with SQLite embedded directly?

3. **Concurrency, Batch Processing & Folder Traversal:**
   - How should folder prompting behave in a web context? (A terminal CLI prompt blocks easily; in a web app, does the UI poll/WebSocket the next queue item, or should this run via a background worker with state flags?)
   - Audio decoding & DSP are CPU-intensive: how to handle thread pooling, decoding libraries (FFmpeg/libavcodec vs. libsndfile), and caching/persistence so re-scanning unchanged tracks is avoided?

4. **Database & Schema Design:**
   - Target database (e.g., SQLite via sqlite3 C++ wrapper) schema to store audio features, track filepaths, genre tags, and fingerprint hashes.
   - Fast index design for sorting and slicing queries (e.g., `ORDER BY bpm DESC LIMIT 10`).

---

### Phase 2: System Architecture & Implementation Blueprint
Following your interrogation points, outline a complete proposed system architecture, including:
- **Data Flow Diagram (ASCII/Text)** from filesystem traversal to audio decoding, DSP feature extraction, SQLite storage, and the web frontend.
- **Component Breakdown:** File scanner & queue manager, DSP pipeline (Essentia / Chromaprint), Storage engine, API layer, and Playlist exporter (M3U generator with absolute/relative path handling).
- **Milestone Plan:** Step-by-step development phases from CLI prototype to complete web application.

Begin immediately with Phase 1.