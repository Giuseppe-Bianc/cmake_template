---
paths:
  - "src/**"
  - "include/**"
  - "fuzz_test/**"
---

# Error Handling (C++)

- Use typed exception types or `std::expected`/`std::optional` over bare `int` error codes or generic `std::runtime_error`.
- Never swallow exceptions silently. Either rethrow with added context or log and propagate.
- Catch by reference (`catch (const std::exception&)`), not by value.
- No exceptions across C ABI / DLL boundaries -- wrap in error codes at the boundary.
- Validate preconditions with `if (!precondition) throw std::invalid_argument(...)` or contracts (`[[expects: ...]]` if toolchain supports it).
- No floating futures / unobserved `std::async`. Every `std::thread`/`std::jthread` joins; every `std::async` is awaited or stored.
- No `std::abort` / `std::terminate` for recoverable conditions. Reserve for invariant violations.
- Resource acquisition: prefer RAII. No manual `new`/`delete`, no manual `close()` calls -- use smart pointers, lock guards, scope guards.
- spdlog: log at `warn` for recoverable errors, `error` for failures, `critical` for invariants. Never log secrets/PII.