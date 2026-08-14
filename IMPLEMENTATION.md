# Crow CLI Implementation

This is the canonical implementation status and backlog for the standalone
Laravel Zero `crow` CLI.

Status labels:

- `[x]` Done in source or documented evidence.
- `[~]` Partially done, modeled, or not fully verified.
- `[ ]` Not done.

## Current Read

The Laravel Zero migration is complete in source. This repository is a
standalone CLI app named `crow`, with command code under `app/Commands`, support
code under `app/Support`, PHAR configuration, a checked-in `builds/crow`
artifact, and feature tests for the primary command flows.

The remaining work is release confidence: refresh the PHAR before publication,
verify Packagist/global Composer installation, decide the production default API
URL, and add release smoke evidence.

## Done

- [x] Converted the repository from a Laravel package into a Laravel Zero 12
  project.
- [x] Composer package identity is `crowbot/cli`.
- [x] The executable entrypoint is `crow`.
- [x] Composer `bin` points at `builds/crow` for PHAR-backed distribution.
- [x] Added Laravel Zero app scaffolding: `bootstrap/app.php`, `config/app.php`,
  `config/commands.php`, `box.json`, and app/service-provider wiring.
- [x] Moved runtime code into the app namespace under `app/Commands` and
  `app/Support`.
- [x] Implemented `crow plan` for listing implementation-plan handoffs, fetching
  a specific plan, JSON output, and file output.
- [x] Implemented `crow read` for latest/specific event reads, unread handling,
  app filtering, and event-type filters.
- [x] Implemented `crow listen` with listener registration, `--no-register`,
  host/port/public URL options, shared secret support, `/crow/events`, and
  `/health`.
- [x] Implemented `crow auth login` with browser launch, token prompt,
  `--api-url`, `--api-token`, `--global`, and `--no-browser`.
- [x] Credentials are written to the nearest project `.crow/config.json` by
  default, with `~/.crow/config.json` used for `--global` or when no project
  root is found.
- [x] Config precedence is explicit option, environment, project config, global
  config, then built-in default.
- [x] Restrictive config directory/file permissions are attempted where the
  platform supports them.
- [x] Existing automation environment variables are preserved, including
  `CROW_API_URL`, `CROW_API_TOKEN`, `CROW_APP_ID`, listener URL/host/port, and
  listener secret.
- [x] Hidden compatibility command names are available for `crow:plan`,
  `crow:read`, and `crow:listen`.
- [x] Feature and unit tests cover plan, read, listen, auth login, config
  precedence, listener behavior, aliases, and API failures.

## Partial

- [~] A PHAR artifact exists at `builds/crow`, but it should be rebuilt and
  smoke-tested from the release commit before publication.
- [~] The default API URL remains `https://crow.test/api/v1`; production release
  should either change it or document the required user override.
- [~] Packagist/Composer global installation is documented, but a dated
  install-from-package smoke report is not recorded here.
- [~] `auth login` exists, but logout/status commands are still future
  ergonomics work.

## Not Done

- [ ] Publish and verify a Packagist/global Composer install.
- [ ] Add dated release smoke evidence for `builds/crow`.
- [ ] Add `crow auth logout`.
- [ ] Add `crow auth status`.
- [ ] Decide and document the production default API URL before public release.
- [ ] Native single-file binaries outside PHAR distribution.

## Next Priority

1. Rebuild the PHAR from a clean release commit with
   `php crow app:build crow --build-version=<version>`.
2. Smoke-test `./builds/crow list`, `./builds/crow plan --help`, auth login
   with `--no-browser`, and one real API-backed plan/read command.
3. Publish or dry-run Packagist installation and verify `composer global require
   crowbot/cli` exposes `crow`.
4. Decide whether the public default API URL should remain `crow.test`, move to
   production, or require explicit setup.
5. Add `auth status` and `auth logout` after release packaging is verified.

## Verification Needed

- [ ] `composer test`
- [ ] `php crow list`
- [ ] `php crow app:build crow --build-version=<version>`
- [ ] `./builds/crow plan --help`
- [ ] `./builds/crow auth login --api-token=<token> --no-browser`
- [ ] `composer global require crowbot/cli` from a published or local package
  source.
