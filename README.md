# sentiment-engine
libobs-based capture/encode engine for Sentiment (IPC control plane)

### Roadmap:
- create component for websocket: will act as the server and listen for requests from the client/app. IPC will be two-way
  - before moving to libobs specific impl, check that ping/auth to server works. optionally also create a set of tests that will do the same thing (ping with no engine functionality just to confirm connection)
- define the Engine Interface Layer (EIL): the level of abstraction we want to expose to the websocket and application (facade design pattern). generic primitive Sentiment engine functions (ie. ArmBuffer, SaveClip, etc.), no libobs types
- read libobs spec and plan what capabilities are needed: implement the Engine Abstraction Layer (EAL) on libobs behind the EIL primitives. Will help avoid issues if backend switches off libobs, or we want fallback engine options (and for debugging and testing).
- hook up the IPC translator so socket requests call the EIL, and engine events get pushed back to the client

---

## Architecture

### Process / Entrypoint
`main`, engine startup/shutdown, config load, logging

### IPC Server
websocket listen, auth/token, JSON request/response, event push to the app

after engine is fully functional, optional plan to add OS-native transfer paths for lower latency (although only file paths and commands are sent, not files so it may not be worth the effort; named pipes, unix sockets).

### IPC Translator Component: IPC <-> Interface Layer
- Retrieve info from socket and translate directly to interface function calls
- Turn engine events back into JSON pushes to the client

---

### Engine Core

#### Engine Interface Layer
Sentiment client primitives the socket calls: ie. arm buffer, save clip, start/stop record, get status, apply settings. No libobs types in the layer
Layer should be thin: headers + a small abstract type (pure virtual / interface) that names Sentiment primitives(ie. SaveClip, ArmBuffer, etc.)

#### Engine Abstraction Layer
Implements facade/primitives from EIL (leaving room for different paths). Primary path and initial implementation will be using libobs (sources, scenes, replay buffer, encoders, file output). Should be able to plug in different recording engines later on.

#### Media / Output Component
Where clips land on disk, naming, basic output metadata returned to the app via websocket

---

### Engine Utils

#### Platform Layer
OS specific helpers/operations. Windows as current priority, implementation should have multiple OS specific paths in mind with future Linux and MacOS support intended.

---

## Tests
tdb
