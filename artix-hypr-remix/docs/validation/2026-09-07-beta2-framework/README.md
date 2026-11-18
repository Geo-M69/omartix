# Beta2 framework delivery evidence — 2026-09-07

Inspected on 2026-11-18 from the live host's AHR state directory. The three
transaction states and associated backup manifests were copied using
`scripts/preserve-framework-transaction-evidence.sh`. Copies replace the host
home directory with `<HOME>`; IDs, timestamps, revisions and other fields are
unchanged. The rollback's associated manifest belongs to the initial apply,
which created that backup. The helper copies the associated manifest; it does
not assert that the rollback created it. The doctor and updater excerpts retain
only campaign-relevant lines. The source records were not modified.

The states and manifests directly show that both applies committed revision
`68b7532c3961ffd4dcc57ffe9ebfbb32574144bf` with the configured public source recorded in the manifests,
and that the intervening rollback completed against the initial apply's backup
with `restore_completed=true`, `doctor_exit=1`, and degraded health. The
initial apply remains `completion=committed`. Its backup's previous revision
was `9d6bef4160c8e3050304ecf61e5d45eda9ba975a`; that revision's doctor treats
Bluetooth as a required service. The captured doctor excerpt identifies the
inactive Bluetooth service as a failure. The recorded rollback smoke log is
empty; it is not an independent baseline-smoke transcript.

The read-only comparison in `snapshot-comparison-and-terminal-inventory.txt`
compares the initial apply's pre-update backup with the final reapply's
post-rollback backup. `diff -qr` found no content differences in the listed
framework targets, metadata, migration snapshot and namespace manifest. This
corroborates restoration of those snapshot contents; it does not independently
prove all filesystem metadata, every symlink target, or live namespace state.
All 16 transaction state files present at inspection reported terminal
completion values. This inventory is an inspection-time observation, not a
complete historical transcript.

The earlier live report additionally stated exact live restoration and that the
final doctor and baseline smoke passed. Those results have no preserved
contemporaneous transcript in this evidence set and remain reported
observations. The state files alone do not establish byte-for-byte historical
immutability of the initial state file; TC65 tests that contract synthetically.

At inspection, HEAD is `68b7532`; tracked candidate changes comprise release and
lifecycle documentation and the TC65 test only, with no runtime or metadata
changes. The evidence files and unrelated pre-existing untracked agent files
are uncommitted. The 520-assertion updater suite and 18-stage `--no-aur`
quality gate passed in the preceding validation; the latter used
`AHR_NO_SUDO=1` in the restricted execution environment. No live mutation was
repeated for documentation and test-only changes. A final release revision must
be reviewed after these files are incorporated; this evidence does not satisfy
the full Phase 3 product exit gate.
