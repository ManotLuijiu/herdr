# Uninstall Official Herdr & Build from Source

How to remove herdr installed via the official `herdr.dev` installer and replace it with a locally-built version from this repo.

## Why Do This?

- You want to use a custom/forked version of herdr
- You need to apply patches or modifications
- You want to develop/contribute to herdr

## Prerequisites

- Rust toolchain installed (`cargo`, `rustc`)
- Git
- sudo access

## Step 1: Find Your Cargo Installation

If `cargo` is not in your PATH, find it:

```bash
# Check common locations
ls ~/.cargo/bin/

# If found, add to PATH (add to ~/.bashrc or ~/.zshrc for persistence)
export PATH="$HOME/.cargo/bin:$PATH"
```

## Step 2: Uninstall Old Herdr

Remove the binary installed by `curl -fsSL https://herdr.dev/install.sh | sh`:

```bash
# Remove the binary
rm ~/.local/bin/herdr

# Remove herdr's data directory (sessions, plugins, config)
rm -rf ~/.config/herdr
```

## Step 3: Clone (If Not Already Done)

```bash
git clone https://github.com/ManotLuijiu/herdr.git ~/herdr
cd ~/herdr
git checkout develop  # or your desired branch
```

## Step 4: Build from Source

```bash
# Make sure cargo is in PATH
export PATH="$HOME/.cargo/bin:$PATH"

cd ~/herdr
cargo build --release
```

Build time: ~2-5 minutes depending on hardware.

## Step 5: Install the Built Binary

```bash
# Copy to your PATH
cp ~/herdr/target/release/herdr ~/.local/bin/herdr
chmod +x ~/.local/bin/herdr
```

## Step 6: Verify

```bash
herdr --version
```

Expected output: `herdr 0.8.0` (or your built version)

## Troubleshooting

### `cargo: command not found`

Rust is not installed. Install it:

```bash
curl --proto '=https' --tlsv1.2 -sSf https://sh.rustup.rs | sh
source ~/.cargo/env
```

### Permission denied when copying

Use `sudo`:

```bash
sudo cp ~/herdr/target/release/herdr ~/.local/bin/herdr
sudo chmod +x ~/.local/bin/herdr
```

### Old herdr still runs after uninstall

Check if there's another `herdr` in your PATH:

```bash
which -a herdr
type herdr
```

Remove any remaining copies and ensure `~/.local/bin` is in your PATH.

## Quick Reference

```bash
# One-liner for experienced users
rm ~/.local/bin/herdr && rm -rf ~/.config/herdr && \
  cd ~/herdr && cargo build --release && \
  cp ~/herdr/target/release/herdr ~/.local/bin/herdr && \
  chmod +x ~/.local/bin/herdr && herdr --version
```
