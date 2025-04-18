# nix-zshrc
*nix zsh development setup

## Overview

This repository (or collection of files) details a personalized Zsh configuration designed for a productive and visually appealing command-line experience. It leverages Zsh's powerful features, integrates various modern command-line tools, and provides a rich set of aliases for common tasks.

Unlike configurations managed by frameworks (like Oh My Zsh) or package managers (like Nix), this setup involves manually placing configuration files and installing dependencies.

**Note:** This configuration seems tailored for macOS (judging by `pbcopy`, `killall Finder`/`Dock`, `~/.local/bin` path comment, Android SDK path structure) but can be adapted for Linux with minor changes (e.g., `pbcopy` alternative like `xclip`, different paths, package manager commands).

## Table of Contents

-   [Features](#features)
-   [Dependencies](#dependencies)
    -   [Core Shell & Tools](#core-shell--tools)
    -   [Zsh Plugins](#zsh-plugins)
    -   [Programming Environment](#programming-environment)
    -   [Optional / Assumed](#optional--assumed)
-   [Installation](#installation)
    -   [1. Install Zsh](#1-install-zsh)
    -   [2. Install Dependencies](#2-install-dependencies)
    -   [3. Install Zsh Plugins](#3-install-zsh-plugins)
    -   [4. Place Configuration Files](#4-place-configuration-files)
    -   [5. Setup Environment Managers (pyenv/nvm)](#5-setup-environment-managers-pyenvnvm)
    -   [6. Set Zsh as Default Shell](#6-set-zsh-as-default-shell)
    -   [7. Launch Zsh](#7-launch-zsh)
-   [Configuration Highlights](#configuration-highlights)
    -   [`.zshrc`](#zshrc)
    -   [`aliases.zsh`](#aliaseszsh)
-   [Customization](#customization)
-   [Notes & Considerations](#notes--considerations)

## Features

* **Sensible Zsh Options:** Enables features like `autocd`, `interactivecomments`, `promptsubst`, and `numericglobsort`.
* **Modern Tool Integration:** Replaces standard commands with enhanced alternatives:
    * `ls` -> `eza` (modern file listing with icons)
    * `cat` -> `bat` (syntax highlighting pager)
    * `grep` -> `ripgrep` (`rg`) (fast recursive search)
    * `top` -> `htop` (interactive process viewer)
    * `ptop`/`bpytop` (advanced process viewer)
    * `gdu`/`gdu-go` (fast disk usage analyzer)
    * `cd` -> Enhanced by `zoxide` (faster directory jumping)
    * `wget` -> `wget2` (potentially)
* **Custom Prompt:** Uses `starship` for a configurable and informative prompt.
* **Fuzzy Finding:** Integrates `fzf` for powerful fuzzy searching in history and files.
* **Command Correction:** Uses `thefuck` for quick correction of typos in previous commands.
* **Version Management:** Ready for `pyenv` (Python) and potentially `nvm` (Node.js).
* **Useful Aliases:** Extensive list covering navigation, file operations, Git, system tasks, network lookups, file editing (`zed`), and local file sharing.
* **Plugin Support:** Includes `zsh-you-should-use` and `fancy-ctrl-z`.
* **Enhanced Completions:** Initializes `compinit` with case-insensitive matching and menu selection.

## Dependencies

This configuration **requires** several external tools and plugins to be installed manually before it will function correctly.

### Core Shell & Tools

* **`zsh`**: The Z shell itself.
* **`starship`**: For the prompt.
* **`zoxide`**: For enhanced `cd` functionality.
* **`fzf`**: For fuzzy finding capabilities.
* **`thefuck`**: For command correction.
* **`eza`**: Replacement for `ls`.
* **`bat`**: Replacement for `cat`.
* **`ripgrep` (`rg`)**: Replacement for `grep`.
* **`htop`**: Replacement for `top`.
* **`bpytop`**: Used by the `ptop` alias.
* **`gdu` / `gdu-go`**: Used by `gdu` and `disk` aliases. Requires `gdu-go` binary specifically.
* **`tree`**: For directory tree view.
* **`wget2`**: Used by `wget` alias (ensure this specific version is intended/installed, or adjust alias). Standard `wget` might suffice if `wget2` isn't available.
* **`nmap`**: For network scanning (`snmap` alias).
* **`curl`**: Used in `ip` alias.
* **`bind-utils` / `dnsutils` (Linux) or built-in (macOS)**: Provides `dig` command (for `dnslookup` alias).
* **`coreutils` (for `ifconfig` on some Linux distros) / built-in (macOS)**: Provides `ifconfig` (for `ip` alias).

### Zsh Plugins

These need to be downloaded and placed in `~/.zsh/`.
* **`zsh-you-should-use`**: Reminds you of existing aliases.
* **`fancy-ctrl-z`**: Enhances `Ctrl+Z` behaviour.

### Programming Environment

* **`pyenv`**: For managing Python versions.
* **`python` / `python3`**: Required for `pyenv` and the `pshare` alias.
* **(Optional) `nvm`**: If you plan to use the `~/.zsh/nvm.zsh` sourcing line, NVM needs to be installed.
* **(Optional) `ruby`**: Required for the `rshare` alias.

### Optional / Assumed

* **`zed`**: The text editor used in editing aliases (`zshrc`, `sship`, `aliases`). You can change these aliases if you use a different editor (e.g., `nvim`, `vim`, `code`).
* **`neovim` (`nvim`)**: Used by the `vim` alias.
* **Android SDK Platform Tools**: The path `~/Library/Android/sdk/platform-tools/` is added to `PATH`. Only needed if you do Android development.

## Installation

Follow these steps to set up the configuration:

### 1. Install Zsh

If Zsh is not already installed:
* **macOS (using Homebrew):** `brew install zsh`
* **Debian/Ubuntu:** `sudo apt update && sudo apt install zsh`
* **Fedora:** `sudo dnf install zsh`

### 2. Install Dependencies

Install **all** the tools listed under [Dependencies](#dependencies) using your system's package manager.

* **macOS (using Homebrew):**
    ```bash
    brew install starship zoxide fzf thefuck pyenv eza bat ripgrep htop bpytop gdu wget tree nmap ruby zed neovim nvm # Add others if needed
    # Note: 'gdu' in Homebrew might install 'gdu-go'. Verify binary name.
    # Note: 'dig' and 'ifconfig' are usually built-in on macOS.
    ```
* **Debian/Ubuntu (using Apt):**
    ```bash
    sudo apt update
    sudo apt install starship zoxide fzf thefuck pyenv eza bat ripgrep htop bpytop gdu wget tree nmap ruby curl bind9utils net-tools # Add others
    # Note: You might need to install 'zed' / 'nvim' / 'nvm' via other methods (check their websites).
    # Note: Check package names carefully (e.g., 'python3-pip' for 'pip', 'golang-gdu' or similar for gdu-go).
    # Note: 'bat' might be 'batcat' on Debian/Ubuntu - create a 'bat' symlink if needed.
    ```
* **Other Systems:** Use the appropriate package manager (`dnf`, `pacman`, etc.).

### 3. Install Zsh Plugins

Download the required Zsh plugins into the `~/.zsh/` directory.

```bash
mkdir -p ~/.zsh
# Install zsh-you-should-use
git clone [https://github.com/MichaelAquilina/zsh-you-should-use.git](https://github.com/MichaelAquilina/zsh-you-should-use.git) ~/.zsh/zsh-you-should-use

# Install fancy-ctrl-z (download the script directly)
curl -o ~/.zsh/fancy-ctrl-z.zsh [https://raw.githubusercontent.com/FigSupport/fancy-ctrl-z/main/fancy-ctrl-z.zsh](https://raw.githubusercontent.com/FigSupport/fancy-ctrl-z/main/fancy-ctrl-z.zsh)
