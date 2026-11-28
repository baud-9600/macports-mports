# MacPorts port overlay

A small, hand-maintained [MacPorts](https://www.macports.org/) port overlay.

The overlay contains only ports that are not already available in the official
MacPorts ports tree, plus a few local variants. Ports that duplicate
the official tree were deliberately removed so that they cannot shadow newer
upstream versions.

## Usage

Clone the repository, index it once, and add it as a local (non-syncing) source:

```sh
git clone https://github.com/baud-9600/macports-mports.git ~/macports-mports
cd ~/macports-mports && portindex

echo "file://$HOME/macports-mports [nosync]" | sudo tee -a /opt/local/etc/macports/sources.conf
```

After editing a Portfile, regenerate the index:

```sh
cd ~/macports-mports && portindex
```

Then install ports as usual, for example:

```sh
sudo port install zensical
sudo port install snapclient
```

## Ports

| Port | Version | Description |
| --- | --- | --- |
| `audio/snapclient` | 0.35.0 | Snapcast multiroom audio client (CoreAudio, Avahi, SSL) |
| `net/xpra` | 6.4.4 | Xpra CLI tools without GTK/GUI dependencies |
| `python/py-zeroconf` | 0.154.0 | Pure-Python mDNS/DNS-SD (subports for Python 3.11–3.14) |
| `python/py-zensical-ui` | 0.0.43 | Templates and browser assets for Zensical (Python 3.11–3.14) |
| `security/monkeysphere` | 0.44 | OpenPGP web of trust for SSH host/user keys |
| `security/spm` | 20230306 | POSIX-shell password manager using rage/age |
| `www/manpageblog` | 1.9 | Static site generator with a man-page layout |
| `www/manpageblog-mk` | 1.9 | Local variant of `manpageblog` with its own port name |
| `www/zensical` | 0.1.3 | Modern static site generator by the Material for MkDocs authors |

## Notes

- **`zensical` uses Python 3.13.** `py-zensical-ui` is available for Python
  3.11–3.14, but the official `py-pymdown-extensions` package only exists for
  Python 3.13/3.14, so the default is 3.13.
- **`zensical` uses the stdlib `tomllib`.** Upstream imports the `tomli`
  backport even though it requires Python ≥ 3.11; MacPorts only ships a
  `py36-tomli` subport, so the port patches the import.
- **`py-zeroconf` requires Python ≥ 3.11** since version 0.154.0.
- **`xpra` is pinned to 6.4.4.** Upstream 7.0 is available, but the port's
  build flags and `setup.py` patches target the 6.4.x tree.
- **`spm` has no tagged upstream releases.** The port fetches commit
  `b56ef81412ba` (last commit on `master`).
- **`monkeysphere` uses `gnupg2`** instead of the EOL `gnupg1` dependency.
- **`manpageblog-mk`** installs the same upstream script as `manpageblog`, but
  under the port name `manpageblog-mk`, so both variants can coexist.
- Excluded duplicates of the official tree: `finance/pta`, `editors/vis`,
  `mail/neomutt`, `python/py-netifaces` (deprecated upstream, Python ≤ 3.11
  only) and `python/py-uvloop` (official port supports Python 3.13/3.14).

## Validation

All ports are checked with:

```sh
port lint --nitpick <port>
port checksum <port>
port livecheck <port>
```

Current status: `port lint --nitpick` reports **0 errors and 0 warnings** for
every port, and all distfile checksums were verified against the live upstream
URLs.

## Related

- FreeBSD/poudriere overlay: <https://github.com/baud-9600/freebsd-mports>
