# apter

**apter** is a lightweight, noise-free helper and porcelain tool for the `apt`, `apt-get`, and `dpkg` infrastructure on Debian and Ubuntu-derived Linux distributions (such as Linux Mint).

It focuses on providing a clean, fast, UNIX-style CLI experience without the clutter, verbose metadata, and localized string variations of raw APT utilities.

---

## Key Features

- **All-Repository Package Search**: `apter search` searches across all repository package names and descriptions by default, highlighting installed packages in green.
- **Shell Glob Expansion**: Native shell glob pattern matching (`*`, `?`, `[...]`) across matching commands (`info`, `search`, `what`).
- **Clean Package Info**: `apter info` strips technical hashes, maintainers, and build metadata, showing only plain-text descriptions alongside `Installed` vs `Candidate` versions.
- **World File Package Tracking**: `apter install` and `apter remove` require exact package names (no wildcards) and track your explicitly installed software in `${XDG_STATE_HOME:-~/.local/state}/apter_world.log` (or `~/.apter_world.log`).
- **Curbed, Deterministic Output**: Package installation and removal operations force `LC_ALL=C -q=2` and stream-filter output to highlight installation/configuration actions cleanly without walls of technical status text.
- **All-Repository File Ownership**: Instantly find which package provides a command or file across all repository packages (`apter who`).
- **Clean History Timeline**: Parses `/var/log/apt/history.log` into an easy-to-read chronological timeline.

---

## Installation

`apter` is a single, self-contained Bash script with no external dependencies beyond standard Debian/Ubuntu core utilities (`apt-get`, `apt-cache`, `dpkg-query`, `bash`, `awk`, `sed`).

```bash
git clone https://github.com/.../apter.git
cd apter
chmod +x apter
sudo cp apter /usr/local/bin/
```

---

## Command Reference

### `apter search <query>`
Searches all repository package names and descriptions.
```bash
apter search clang               # Search for packages matching 'clang'
apter search 'clang-*'           # Search using shell glob pattern matching
```

### `apter info <package|glob...>`
Displays clean description and version status (`Installed` vs `Candidate`).
```bash
apter info clang                 # Info for clang package
apter info 'clang-1?'            # Info for all matching glob packages (clang-14..clang-19)
```

### `apter install [apt-options] <package...>`
Installs exact packages and records them in your `world` file. Wildcards disallowed.
```bash
apter install sl htop            # Install packages and track in world file
apter install -y neovim          # Pass options directly to apt-get
```

### `apter remove [apt-options] <package...>`
Removes exact packages and prunes them from your `world` file.
```bash
apter remove sl                  # Uninstall package and remove from world file
apter purge sl                   # Uninstall package and purge configuration files
```

### `apter world`
Lists all packages tracked in the world log file (`apter_world.log`), formatted into columns.
```bash
apter world                      # Show tracked packages in world log
```

### `apter update`
Quietly updates the APT package index.
```bash
apter update
```

### `apter upgrade [apt-options]`
Displays a clean summary of upgradable packages (`Installed -> Candidate`) before prompting to upgrade.
```bash
apter upgrade
```

### `apter orphans [options]`
Lists or cleans unneeded auto-installed dependency packages.
```bash
apter orphans                    # List orphaned dependency packages
apter orphans --clean            # Cleanly remove orphaned packages (or 'apter autoremove')
```

### `apter who <file|command...>`
Finds which package provides a given file or command across all repository packages (using `apt-file`).
```bash
apter who /usr/bin/gcc           # Returns package 'gcc'
apter who bash                   # Resolves command in $PATH and returns package 'bash'
```

### `apter what <package|glob...>`
Lists all files installed by specified package(s). Supports shell glob patterns.
```bash
apter what adduser               # List files installed by adduser
apter what 'gcc-*'               # List files installed by matching installed packages
```

### `apter history [options]`
Displays a clean timeline of past package operations from `/var/log/apt/history.log`.
```bash
apter history                    # Show last 10 package operations
apter history -n 5               # Show last 5 package operations
```

---

## License

Dedicated to the public domain under the [CC0 1.0 Universal License](LICENSE.md).
