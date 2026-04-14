# Architecture Taxonomy

> **Stub file.** Replace with your project's actual layers, boundaries, and ownership.

## Layers

Describe the architectural layers of this system. Example taxonomy:

- **Engine** — the core algorithms and domain logic. Pure, testable, no I/O.
- **Platform** — cross-cutting concerns: auth, storage, observability, messaging.
- **Application** — user-facing surfaces (web app, mobile app, CLI, API).

## Boundaries

Where each layer ends and the next begins. Explicit contracts at each seam.

## External Dependencies

Third-party services, APIs, and MCP servers (cross-reference `MCP_SERVERS.md`).

## Owners

Which team or person is accountable for each layer.

## Non-goals

Things this architecture deliberately *isn't* trying to do. Protects against scope creep.
