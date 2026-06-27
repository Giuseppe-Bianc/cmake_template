# cmake_template

C++23 CMake template. MSVC / clang-cl / gcc / clang / Emscripten.

## Commands

- Configure: `cmake --preset <name>` (presets in `CMakePresets.json`: `windows-msvc-debug`, `unixlike-gcc-debug`, `unixlike-clang-debug`, ...)
- Build: `cmake --build out/build/<preset>`
- Test: `ctest --preset <preset>` (or `ctest -R "unittests|relaxed_constexpr" --output-on-failure`)
- Format: `clang-format -i <changed>.cpp <changed>.hpp`
- Lint: project enables clang-tidy + cppcheck on top-level builds

## Build presets (top-level)

`myproject_PACKAGING_MAINTAINER_MODE=OFF`, `myproject_ENABLE_COVERAGE=ON`,
`myproject_ENABLE_CLANG_TIDY=ON`, `myproject_WARNINGS_AS_ERRORS=ON` are default
when configuring from the project root.

## Project layout

- `src/` -- library + executable targets (each subdir has its own `CMakeLists.txt`)
- `include/` -- public headers (namespace `myproject`)
- `test/` -- Catch2 v3 tests (`tests.cpp`, `constexpr_tests.cpp`)
- `fuzz_test/` -- libFuzzer harness
- `cmake/` -- CMake helpers (sanitizers, hardening, IPO, etc.)
- `Dependencies.cmake` -- CPM-managed deps (fmt, spdlog, Catch2, CLI11, FTXUI)

## Rules

- Read `AI_GUIDELINES.md` for project-specific C++/CMake standards.
- Never disable warnings, sanitizers, clang-tidy, or cppcheck -- fix the issue.
- Never rewrite entire files; make targeted edits only.
- No em dashes -- use double hyphens.