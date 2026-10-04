# TOFIX

Findings from a code scan on 2026-10-04.

## High

- `client.sh:4-5` - `pip install --user` fails on current Debian/Ubuntu (PEP 668 "externally-managed-environment"), and the script reinstalls (with `--force`) on every run. `mssql-cli` itself is retired by Microsoft (repo archived, replaced by `sqlcmd` / go-sqlcmd). Switch the client to `sqlcmd -S localhost -U sa` and move installation out of the run script (README instructions, or `uv tool install` if a Python tool is kept).

## Medium

- `client.sh:8,11` and `server.sh:5` - the SA password is hardcoded in both scripts and passed with `-P` on the command line (visible in `ps`). Keep one definition: read it from an environment variable (e.g. `MSSQL_SA_PASSWORD`) set by the user or fetched with `pass show`, and let the client use the `SQLCMDPASSWORD` env var instead of `-P`.
- `server.sh:3-7` - not re-runnable: a second run fails because container `sqlserver1` already exists. Add `docker rm -f sqlserver1 2>/dev/null` first or `docker start` an existing container, and add a matching stop/remove script.
- `README.md:1-2` - the repo is described as "Demos for the Microsoft SQL Server product" but has no SQL demos, only server/client launch scripts, and the README does not explain how to use them. Add usage (run `server.sh`, then `client.sh`) and either add the demos or reword the description.

## Low

- `client.sh:11` - trailing whitespace at end of line.
- `notes_for_sql.txt:2` - says "sql server 2019 is the main system to teach" while `server.sh:7` runs `2022-latest`; reconcile.
