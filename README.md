# sentiment-engine
libobs-based capture/encode engine for Sentiment (IPC control plane)

### Roadmap:
- create component for websocket: will act as the server and listen for requests from the client/app. IPC will be two-way
  - before moving to libobs specific impl, check that ping/auth to server works. optionally also create a set of tests that will do the same thing (ping with no engine functionality just to confirm connection)
- read libobs spec and plan what capabilities are needed: establish translation layer from libobs functions to generic primitive Sentiment engine functions. Will help avoid issues if backend switches off libobs, or we want fallback engine options (and for debugging and testing).
- develop middle layer of engine to create the level of abstraction we want to expose to the websocket and application (facade design pattern)
