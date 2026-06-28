---
alwaysApply: true
---

# Code Quality

## Anti-defaults (counter common Claude tendencies)

- No premature abstractions. Three similar lines beats a helper used once.
- Don't add features or improvements beyond what was asked.
- Don't refactor adjacent code while fixing a bug.
- No dead code or commented-out blocks. Git has history.
- WHY comments, never WHAT. If code needs a "what" comment, rename instead.
- API docs at module boundaries only, not every internal function.

## C++23 conventions

- C++ standard: 23, no compiler extensions (`CMAKE_CXX_EXTENSIONS OFF`).
- Default to `constexpr` / `consteval`. Mark functions `[[nodiscard]]` when the return value matters.
- Prefer strong types over primitives. Use concepts to constrain templates.
- `std::format` / `std::print` instead of iostream / printf. No raw `new`/`delete`.
- Prefer `std::unique_ptr` over `std::shared_ptr`. Apply the Rule of 0.
- Use standard algorithms and ranges; ranged-for with `auto` for the rest.
- Scoped enums (`enum class`). `if constexpr` for compile-time branches. Avoid `default` in `switch` when exhaustiveness matters.

## Naming

- Files: snake_case (`sample_library.cpp`, `fuzz_tester.cpp`). Headers in `include/` use snake_case too.
- Classes / structs: `PascalCase`. Functions / variables: `snake_case`. Constants: `kPascalCase` or `SCREAMING_SNAKE`. Namespaces: lowercase (`myproject`).
- Booleans: `is_` / `has_` / `should_` / `can_` prefix. Functions: verb-first.
- Abbreviations only when universally known (`id`, `url`, `api`).

## Code markers

`TODO(author): desc (#issue)` for planned work. `FIXME(author): desc (#issue)` for known bugs. `HACK(author): desc (#issue)` for ugly workarounds (explain the proper fix). `NOTE: desc` for non-obvious context. Owner and issue link required. Never `XXX`, `TEMP`, `REMOVEME`.

## File organization

- Header order in source files (clang-format `IncludeBlocks: Preserve`): paired header first, then `<vector>`/`<string>`/stdlib, then third-party (fmt, spdlog, ...), then project headers (`#include "myproject/..."`), then relative.
- One class per header. Headers in `include/myproject/`. Implementations in `src/<component>/`.
- Function order: public API first, then helpers in call order.
- CMake target naming: `myproject::<component>`. Use `myproject::myproject_options` and `myproject::myproject_warnings` interface libs.