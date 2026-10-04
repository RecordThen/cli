# RecordThen Crow CLI

These instructions extend `../AGENTS.md` for this independent Git repository.
Read `README.md` for usage and `IMPLEMENTATION.md` for current migration/release
status.

## Ownership

This is the standalone Laravel Zero CLI distributed as the `crow` executable
and `crowbot/cli` Composer package. It owns:

- `crow auth login` and local configuration discovery;
- `crow plan`, `crow read`, and `crow listen`;
- terminal-friendly markdown/JSON output and listener delivery;
- PHAR construction and command compatibility aliases.

It is not the installable Laravel package in `../packages/listen`. Keep shared
behavior compatible where appropriate, but do not restore host-application
service-provider assumptions here.

## Important Paths

- `app/Commands/` — user-facing Laravel Zero commands.
- `app/Support/` — API, configuration, formatting, and listener behavior.
- `config/` — application and Crow environment configuration.
- `tests/Feature/`, `tests/Unit/` — command and support coverage.
- `box.json` — PHAR build configuration.
- `builds/crow` — generated release executable referenced by Composer.

## Conventions and Security

- Preserve short commands as the primary interface and compatibility aliases
  when changing command registration.
- Keep output useful in interactive terminals, pipes, and AI-agent handoffs.
  Send diagnostics to stderr and structured/requested output to stdout.
- Maintain configuration precedence documented in `README.md`.
- Project and global config files may contain API tokens. Never print tokens,
  commit local `.crow` configuration, or include credentials in fixtures.
- Keep HTTP behavior compatible with the versioned Laravel API and test failure,
  timeout, authentication, and malformed-response paths.
- Do not hand-edit the PHAR. Rebuild it only through the Laravel Zero build
  command, and commit a refreshed artifact only as part of an intentional
  release.

## Verification

Run from `cli/`:

```bash
composer install
composer test
php crow list
php crow app:build crow --build-version=unreleased
./builds/crow plan --help
```

Routine code changes need the focused or full test suite. Run PHAR build and
artifact smoke checks for command registration, packaging, dependency, or
release changes. Keep `IMPLEMENTATION.md` aligned with actual status.
