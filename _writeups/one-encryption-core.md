---
title: "Core + Clients"
order: 3
group: marginborn
tags: ["Security", "Architecture"]
problem: "Three clients must encrypt and decrypt the same notes, with no drift between them."
decision: "One Rust crate implements the encryption for every client: exposed to Swift and Kotlin over UniFFI, and compiled to WASM for the web."
rejected: "A native crypto implementation on each platform. A user must be able to unlock a note on any device."
sources: [WL-004, WL-016, WL-021, WL-025, E10-001]
---

## Context

Marginborn uses zero-knowledge encryption: notes are encrypted on the device (AES-GCM-256, WebAuthn PRF), and the server sees user content only as ciphertext and wrapped keys. The same note must open on iOS, Android and the web.

The claim has a limit. It covers sync and storage. When a user runs an AI feature, the AI path sees the plaintext that the user sends.

## What I weighed

| Option | For | Against | Verdict |
|---|---|---|---|
| A native crypto implementation on each platform | Each platform's own libraries | Three implementations can drift, and a note sealed on one device may not open on another | Rejected |
| One Rust core for every client | One implementation of the primitives | One more toolchain to maintain (UniFFI, WASM) | Chosen |

## Decision

braird-core is one Rust crate, exposed over UniFFI to Swift and Kotlin and compiled to WASM. It is the single implementation of the end-to-end encryption primitives that every client shares. CI enforces byte-for-byte parity with the reference implementation against frozen cross-client test vectors.

Each encrypted note is also bound to its record: the note's id is AES-GCM additional authenticated data, so a valid ciphertext moved to another row does not decrypt. The same binding later covered book URLs and embeddings.

The code is public: [braird-core](https://github.com/braird/braird-core).

## Lesson

One audited core beats three re-implementations that drift silently. Encrypt the content and bind it to its place; a valid ciphertext in the wrong row is still tampering.
