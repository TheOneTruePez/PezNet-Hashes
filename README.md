# PezNet public hashes

This repository holds **only** the fingerprints of PezPC's root keys and the
SHA-256 of every PezNet installer script, so you can check what you download
against something PezPC publishes somewhere other than the server you
downloaded it from.

A key inside `install.sh` cannot protect you from the server that served
`install.sh`, so check the script itself first, here.

**Entries start with the first PezNet release signed by PezPC's root keys.**
Until then both lists are empty.

## Check an installer before you run it

Download `install.sh` (Linux) or `Install.ps1` (Windows) from your PezNet
download link, then hash it.

Linux:

```bash
sha256sum install.sh
```

Windows (PowerShell):

```powershell
(Get-FileHash Install.ps1 -Algorithm SHA256).Hash.ToLower()
```

Find the script's version and build at the top of the file (`VERSION` and
`BUILD_NUMBER` in `install.sh`, `$Version` and `$BuildNumber` in `Install.ps1`)
and compare the hash with the matching line in [`bootstraps.txt`](bootstraps.txt).
**If it is not listed, or does not match, do not run it**, and report it
privately (see [`SECURITY.md`](SECURITY.md)).

Once the installer is checked, it checks everything else for you: it verifies
PezPC's signed release statement before it runs the installer engine, and the
engine verifies every release back to PezPC's root keys before it installs or
updates anything.

## Check the root keys a server trusts

Settings > Updates on any PezNet install lists the root keys it trusts, with
their SHA-256 fingerprints. They must match [`roots.txt`](roots.txt). The
same file is what the manual re-anchor on the server console asks you to
compare against, if PezPC ever has to replace its roots.

## What is here

| File | What |
|---|---|
| `roots.txt` | PezPC's root keys: key id, SHA-256 fingerprint of the raw Ed25519 public key, and its role (primary or cold). Release keys' P-256 fingerprints too. |
| `bootstraps.txt` | One line per published installer: file, version, build, SHA-256. |

Nothing else is published here.

## How this repository is protected

- Every commit on `main` is signed and shows as **Verified** on GitHub.
- History is append only. Repository rules refuse force pushes, deleting
  `main`, any other branch and any tag, with no bypass for anyone. Anyone
  can read the rules on the repository's Rules page, and an entry you
  checked once stays in the history for good: if a line you compared
  before has changed or gone, treat that as an attack and report it.
- No GitHub Actions, issues, wiki or projects, so nothing runs here and
  nobody can post look-alike hashes beside the real ones.
- Files are stored byte for byte (`.gitattributes`), so a clone on any
  system hashes the same.
