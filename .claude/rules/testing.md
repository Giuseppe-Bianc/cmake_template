---
alwaysApply: true
---

# Testing

- Framework: Catch2 v3 (`TEST_CASE`, `SECTION`, `REQUIRE`, `STATIC_REQUIRE`).
- Three targets: `tests` (runtime REQUIRE), `constexpr_tests` (STATIC_REQUIRE, compile-time), `relaxed_constexpr_tests` (runtime STATIC_REQUIRE for debugging constexpr tests).
- TDD workflow: add to `relaxed_constexpr_tests` first when constexpr is possible, debug there, then ensure `constexpr_tests` still compiles. Use `tests` only for runtime-only code.
- Verify behavior, not implementation. Don't assert mock call counts when output values would do.
- Run the specific test file after changes, not the full suite. Faster feedback, fewer tokens.
- Flaky test? Fix it or delete it. Never retry to make it pass.
- Prefer real implementations. Mock only at system boundaries (network, filesystem, clock, randomness).
- One assertion per test. Test names describe behavior. Arrange-Act-Assert. No `if` or loops in tests.
- Never `REQUIRE(true)` or check a mock was called without verifying arguments.
- For fuzz code: harness lives in `fuzz_test/fuzz_tester.cpp`, requires ASan or TSan + LibFuzzer-capable compiler.
- Never filter tests when running test binaries -- you need to know if the feature broke other tests.