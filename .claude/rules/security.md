---
paths:
  - "src/**"
  - "include/**"
  - "fuzz_test/**"
  - "**/CMakeLists.txt"
  - "Dependencies.cmake"
  - "ProjectOptions.cmake"
  - "cmake/**"
---

# Security (C++ / native)

- Validate all external input at the trust boundary (parsers, FFI, network, file loaders). Use strongly-typed wrappers, not raw `std::string`/`char*`.
- Never trust size/length from untrusted sources for `memcpy`/`std::copy`/slicing. Bounds-check first.
- Format strings: use `std::format` / `fmt::format`. Never `printf` with user-controlled format. Never pass user data as the format string.
- No raw SQL / shell. This project has no DB layer; if one is added, use parameterized queries / library escaping.
- Use constant-time comparison for secrets and tokens (`crypto_memcmp`-style), not `==`.
- Never log secrets, tokens, passwords, or PII. spdlog/fmt should redact these at the call site.
- For the WASM/Emscripten build: the `web/` directory serves the bundle. Same rules apply -- no secrets embedded in the WASM payload.
- Fuzz harness (`fuzz_test/`) must not panic on arbitrary input; use libFuzzer's `LLVMFuzzerTestOneInput` signature and cap allocations.
- Treat compiler warnings as errors (`myproject_WARNINGS_AS_ERRORS=ON` by default). Address the root cause; never suppress a warning to make code ship.