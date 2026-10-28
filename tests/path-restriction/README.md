# Tests for the path restriction (rule 9507090)

These tests need `tx.wordpress-rule-exclusions-plugin_path` to be set for the
test hosts, which the regular CI cannot do: the `crs-config` input of
crs-plugin-test-action is written to `/tmp/crs-additional-setup.conf` but not
mounted into the containers. They are therefore kept out of
`tests/regression` and run manually:

1. Use the same containers as crs-plugin-test-action
   (`tests/integration/docker-compose.yml` there).
2. Mount a plugin directory that contains `plugins/*.conf` of this repository
   plus `000-path-test-config.conf` from this directory.
3. Run go-ftw against both backends:
   `ftw run -d tests/path-restriction` (and `-d tests/regression`).

Last run: 2026-10-29, CRS v4.25.2, owasp/modsecurity-crs 4-apache-202607160307
(ModSecurity v2) and 4-nginx-202607160307 (libmodsecurity v3), go-ftw v2.4.0:
all tests passed on both backends.
