# Contributing to Kizō

Kizō is free, open-source software for small businesses — built by a community of operators, developers, and local professionals. Contributions of every size are welcome, whether you're fixing a typo, adding a payment-processor adapter, or launching a whole new sector.

## How Kizō is organised

Kizō is a multi-repo project organised in three tiers:

1. **Kernel (`kizo-core`)** — small and stable. Provides the event bus, the module registry, shared domain types, and design tokens. Most contributors never need to touch this.
2. **Modules** — independent PWAs (Register, Orders, Books, Stock, Guests, Team, Pulse, Help). Each module is a standalone application with its own database and entry point. A module never imports another module; communication flows through named events on the event bus.
3. **Sectors** — `kizo-food`, `kizo-beauty`, `kizo-auto`, etc. Each sector composes a curated set of modules, adds sector-specific workflows and compliance, and is its own open-source project with its own contributor community.

A module must run on its own. Disabling, removing, or forking any module must not break the rest of the system.

## Where to contribute

| If you want to… | Open a PR against… |
| --- | --- |
| Fix the website or landing page | `getkizo/getkizo.com` |
| Improve the kernel, event bus, or shared types | `getkizo/kizo-core` |
| Improve a module that ships in Food | `getkizo/kizo-food` |
| Adapt Kizō for a new sector | Fork a sector repo, or open an issue on `kizo-core` to discuss a new one |
| Add a payment-processor or marketplace adapter | The relevant sector repo (e.g. `kizo-food/adapters/<name>`) |

Not sure where something belongs? Open an issue on `kizo-core` and we'll point you to the right repo.

## Getting started

1. **Fork** the repo you want to contribute to on GitHub.
2. **Clone** your fork and create a topic branch off `main`:
   ```bash
   git checkout -b feat/short-description
   ```
3. **Make your changes** following the conventions below.
4. **Run the tests** for the repo you're working in. Each repo's README explains how.
5. **Open a pull request** against `main` with a clear title and description. Reference any related issue.

## Conventions

### No direct imports between modules

Modules must never `import` another module. Inter-module integration happens through named events on the event bus. For example, when Register emits `order.completed`, Books, Stock, and Guests listen independently — none of them know Register exists.

This is the rule that lets any module be disabled, replaced, or independently forked without breaking the system. PRs that violate it will be asked to refactor.

### Each module is standalone

Every module ships as a standalone PWA with its own entry point and database connectivity. It must be runnable on cheap hardware (a Raspberry Pi is the reference target) without the sector shell or any other modules present.

### Commit messages

We use Conventional Commits:

```
feat(register): add split-check support
fix(books): correct VAT rounding on refunds
docs(merchant): add Manager App user manual
chore: bump dependencies
```

Use the scope of the module or area you're changing.

### Code style

Repo-specific conventions live in each repo's `README.md` and `.editorconfig`. In general:

- **Naming:** camelCase for variables and functions, PascalCase for classes and types.
- **Comments:** JSDoc for public APIs. Inline comments only when the *why* is non-obvious.
- **Errors:** throw exceptions; do not swallow them silently.
- **Tests:** TDD is encouraged — write the test first when fixing a bug or adding behaviour.

### Documentation

If your change affects how an operator uses Kizō, update the relevant user manual (in `kizo-food/apps/merchant/USER_MANUAL_*.md` for Food). If your change affects how a developer integrates with Kizō, update the relevant repo's `README.md` or `docs/`.

## Reporting bugs and requesting features

- **Bugs:** open an issue on the repo where the bug lives. Include reproduction steps, expected vs actual behaviour, and your environment.
- **Features:** open a discussion first (Reddit `r/getkizo` or a GitHub issue) so we can talk through the design before you spend time implementing.

## Community

- **Reddit:** [r/getkizo](https://www.reddit.com/r/getkizo/) — the main community channel for questions, feedback, and feature discussion.
- **GitHub Discussions:** for design proposals tied to a specific repo.

## License

By contributing, you agree that your contributions will be licensed under the same licence as the repo you're contributing to. Kizō repos are MIT-licensed unless stated otherwise in the repo's `LICENSE` file.

---

寄贈 — *a gift with no expectation of return.* Thank you for helping build it.
