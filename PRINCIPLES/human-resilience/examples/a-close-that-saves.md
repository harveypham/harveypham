# A Close That Saves

<p align="right">Refined with assistance of AI tools</p>

A teammate once built a library for loading a large free‑form tree into memory, editing it, and writing it back to disk. The API exposed four methods: *Open* (load data from a file object and associate it with the in‑memory structure), *IsModified*, *Save* (persist changes), and *Close* (free memory and close the file).

Over time, users noticed that every workflow required calling *IsModified*, then *Save*, then *Close*. Based on that pattern, the developer proposed folding *Save* into *Close*. The updated *Close* became “smart”: it checked whether the data had been modified and saved automatically when needed.

Years later, we encountered a scenario where we wanted to modify the in‑memory data but not save it. By then, the API was mature and widely used, so changing it would have required updates across multiple components. Instead, we implemented a file‑like abstraction that supported a “close without saving” behavior and plugged that into the existing API.

----

Originally, *Save* and *Close* were separate operations, giving users the freedom to adapt to different situations: save without closing, close without saving, or discard changes. When the two were merged for convenience, that freedom disappeared. This violated the principle that systems should remain simple enough for human resilience to function.

A later workflow required “modify but don’t save,” and the system could no longer express that intent. Developers had to invent a file‑like object that discards changes on close — a workaround that restored the flexibility the system had removed. This less elegant solution shows human resilience at work in the face of a rigid system.