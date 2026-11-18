# Current beta2 host health — 2026-11-18

At approximately 08:20 EST, the installed `ahr-doctor` and repository
`scripts/post-install-smoke.sh --user <USER>` ran with access to the host
process namespace (PID 1: `init`). Both exited 0. The complete output is
archived here with the home directory and user name replaced by placeholders.
These are current checks, not transcripts of the September campaign.

Both checks warn that optional Bluetooth is not running and four extended MIME
handlers are unset; SwayOSD is absent. Doctor additionally warns that optional
printing services are enabled but stopped. Smoke used its default printing
mode, which does not infer the profile, and confirmed the live Hyprland session
and required services. An initial sandboxed attempt could not see host
processes and reported required services stopped; those results are not host
health evidence.

The installed framework metadata reports version `0.2.0-beta2`, channel `beta`,
and revision `68b7532c3961ffd4dcc57ffe9ebfbb32574144bf`. Read-only `diff -qr`
checks of `bin`, `migrations`, `docs`, `hooks`, `first-run.d`, and `default`
against the validated checkout found no content differences. This comparison
does not establish all file metadata or symlink identity. All 16 transaction
state files had terminal completion values when inspected. These checks did not
modify installed framework files, services, or transaction records.
