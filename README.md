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
-   [Usage Examples](#usage-examples)

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
```

### 4. Place Configuration Files
Copy the content of the provided .zshrc file to your user's home directory: ~/.zshrc.
Create the ~/.zsh/ directory if it doesn't exist (it should exist after step 3).
Copy the content of the provided aliases.zsh file to ~/.zsh/aliases.zsh.
(Optional) If you have custom functions or NVM setup, place them in ~/.zsh/functions.zsh and ~/.zsh/nvm.zsh respectively.

### 5. Setup Environment Managers (pyenv/nvm)
Ensure pyenv (and nvm if used) are correctly set up beyond just the eval lines in .zshrc. This often involves adding initialization lines to your shell profile as per their official documentation. The eval "$(pyenv init -)" line handles much of pyenv's setup. For nvm, follow its install script instructions carefully.

### 6. Set Zsh as Default Shell
Make Zsh your login shell:
```bash
chsh -s $(which zsh)
```

### 7. Launch Zsh
Open a new terminal window. If everything is installed and configured correctly, you should see the Starship prompt, and the aliases and features should be active.

### Configuration Highlights
#### .zshrc
Entry Point: This is the main file Zsh reads when starting an interactive session.
Options (setopt ...): Configures various Zsh shell behaviours.
Plugin Loading (source ...): Loads the manually installed plugins. Note that zsh-autosuggestions is explicitly not loaded, potentially assuming usage of a terminal like Warp that provides similar functionality.
Sourcing Local Files: Loads aliases, functions, and nvm configuration from the ~/.zsh/ directory.
Completion System (compinit, zstyle ...): Initializes and configures Zsh's tab completion.
Environment (export ...): Sets environment variables like BAT_THEME and modifies the PATH.
Tool Initialization (eval ...): Runs initialization commands required by tools like starship, zoxide, fzf, thefuck, and pyenv to integrate them into the shell session.
#### aliases.zsh
Shortcuts: Provides short aliases for frequently used commands (c, h, ga, gs, etc.).
Safety/Verbosity: Makes mv and cp verbose and interactive (-vi).
Modern Replacements: Aliases standard commands to their modern counterparts (ls->eza, cat->bat, grep->rg, etc.).
Navigation: Quick cd .. aliases (.., ..., .2, etc.).
Editing: Aliases for quickly editing configuration files using zed (zshrc, sship, aliases). Remember to change zed if you use a different editor.
Utility: Includes aliases for IP lookup (ip), local file sharing (pshare, rshare), and network scanning (snmap).

### Customization
Aliases: Add, remove, or modify aliases by editing the ~/.zsh/aliases.zsh file.
Zsh Options: Add or change setopt lines in ~/.zshrc.
Plugins: Add new plugins by cloning/downloading them (often into ~/.zsh/) and adding a source line in ~/.zshrc. Remove plugins by deleting the corresponding source line and removing the plugin files.
Tools: Change which tools are used (e.g., change eza back to ls, change the editor in aliases) by modifying ~/.zsh/aliases.zsh. Remember to install/uninstall the corresponding dependency.
Starship Prompt: Customize the prompt by editing Starship's configuration file, typically located at ~/.config/starship.toml. The sship alias provides a shortcut if you use zed.
Environment Variables: Modify export lines in ~/.zshrc.
After making changes to .zshrc or files sourced by it (like aliases.zsh), either restart your shell or run source ~/.zshrc (or use the sc alias) to apply the changes to your current session.

### Notes & Considerations
Editor: The configuration uses zed in several aliases. If you prefer nvim, vim, code, or another editor, update the zshrc, sship, and aliases aliases in ~/.zsh/aliases.zsh.
zsh-autosuggestions: This popular plugin is explicitly commented out in the .zshrc, with a note suggesting it's unnecessary when using Warp Terminal. If you are not using Warp (or a similar terminal with built-in suggestions), you might want to install zsh-autosuggestions and uncomment/add its source line.
zsh-z vs zoxide: The .zshrc notes that zsh-z was replaced with zoxide (initialized via eval "$(zoxide init zsh)").
ifconfig: The ip alias uses ifconfig. On modern Linux systems, ifconfig is often deprecated in favor of ip addr. The alias might need adjustment depending on your OS and installed tools (ip addr show | grep "inet\s" might be a replacement part). macOS commonly still uses ifconfig.
Error Handling: If commands fail after setup, it's likely due to a missing dependency. Revisit the Dependencies list and ensure everything is installed correctly. Check the output when opening a new terminal for any error messages.

## Usage Examples

This section provides examples of how to use the aliases and integrated tools configured in this setup.

### Standard Aliases

* **Clear Screen:**
    ```bash
    # Instead of typing 'clear'
    c
    ```

* **View History:**
    ```bash
    # Show command history
    h

    # Search history for lines containing 'docker'
    gh docker
    ```

* **File/Directory Operations:**
    ```bash
    # Create nested directories without errors if they exist
    mkdir my/new/project/src

    # Move file, prints action, asks before overwrite
    mv old_name.txt new_name.txt
    # Output might be: renamed 'old_name.txt' -> 'new_name.txt'

    # Copy file, prints action, asks before overwrite
    cp config.yaml config_backup.yaml
    # Output might be: 'config.yaml' -> 'config_backup.yaml'

    # Display directory structure with colors
    tree
    ```

* **System Information:**
    ```bash
    # Ping google.com 5 times
    ping google.com

    # Show disk usage in human-readable format
    df
    ```

* **Clipboard (macOS):**
    ```bash
    # Copy the content of a file to the clipboard
    cat file.txt | pbc
    ```

* **Editor:**
    ```bash
    # Open a file in Neovim (nvim)
    vim my_script.py
    ```

* **GPG Keys:**
    ```bash
    # List your GPG keys
    gpgl
    ```

### Python Aliases

* **Run Python Scripts:**
    ```bash
    # Run script using default python (managed by pyenv)
    p main.py

    # Run script specifically with python 2 (if configured in pyenv)
    # Ensure $HOME/.pyenv/shims/python2 exists and points to a pyenv-managed Python 2
    p2 legacy_script.py

    # Run script specifically with python 3
    p3 script_py3.py
    ```

### macOS Specific Aliases

* **Restart Finder/Dock:**
    ```bash
    # Restart the Finder process
    rfinder

    # Restart the Dock process
    rdock
    ```

### Modern Tool Replacements (Aliases)

* **View File Content (`cat` -> `bat`):**
    ```bash
    # Display a file with syntax highlighting, no pager for small files
    cat my_code.js
    ```
    *Output will be colorized based on the file type.*

* **Disk Usage (`gdu`/`disk` -> `gdu-go`):**
    ```bash
    # Analyze disk usage of the current directory interactively
    gdu
    # Or use:
    disk

    # Analyze a specific directory
    gdu /var/log
    ```
    *This opens an interactive terminal UI to navigate and see disk usage.*

* **Process Monitoring (`ptop` -> `bpytop`, `top` -> `htop`):**
    ```bash
    # Launch the bpytop process monitor
    ptop

    # Launch the htop interactive process monitor
    top
    ```
    *Both open full-screen interactive interfaces.*

* **Search (`grep` -> `rg`):**
    ```bash
    # Recursively search for 'myVariable' in the current directory
    grep 'myVariable' .

    # Search for 'ERROR' within a specific file
    grep 'ERROR' application.log
    ```
    *`rg` is generally much faster than standard `grep`, especially for recursive searches.*

* **Download (`wget` -> `wget2`):**
    ```bash
    # Download a file (using wget2 if available)
    wget [https://example.com/large_file.zip](https://example.com/large_file.zip)
    ```

### Network Aliases

* **DNS Lookup (`dnslookup` -> `dig`):**
    ```bash
    # Show concise A/AAAA records for a domain
    dnslookup example.com
    ```
    *Output will show the IP address(es) associated with the domain.*

* **IP Information (`ip`):**
    ```bash
    # Display local IP addresses and your public IP address
    ip
    ```
    *Combines output from `ifconfig` (local) and `curl ipinfo.io/ip` (external).*

* **Stealthy Scan (`snmap` -> `nmap`):**
    ```bash
    # Perform a specific type of slow, fragmented nmap scan
    snmap target-host.com
    ```
    *Note: Use network scanning tools responsibly and ethically.*

### Git Aliases

* **Stage Changes:**
    ```bash
    # Stage all changes in the current directory
    ga .

    # Stage a specific file
    ga README.md
    ```

* **Check Status:**
    ```bash
    # Show git status
    gs
    ```

* **Commit Changes:**
    ```bash
    # Commit staged changes with a message
    gc -m "Implement new feature"
    ```

### File Listing (`ls` -> `eza`)

* **Basic Listing:**
    ```bash
    # List files with icons, directories first (using eza)
    ls
    ```
    *Output will look different from standard `ls`, often more colorful and using icons if your terminal supports them.*

* **Long Format / Hidden Files:**
    ```bash
    # Long format, human-readable sizes, icons, dirs first
    ll

    # List all files including hidden, icons, dirs first
    la

    # Long format, human-readable, all files including hidden, icons, dirs first
    lla
    # or
    lsa
    ```

### Navigation Aliases

* **Quickly Move Up Directories:**
    ```bash
    # Go up one directory (equivalent to cd ..)
    ..

    # Go up two directories (equivalent to cd ../..)
    ...
    # or
    .2

    # Go up three directories
    ....
    # or
    .3
    ```

### Dotfile Editing Aliases (`zed`)

* **Reload Zsh Config:**
    ```bash
    # Source ~/.zshrc in the current shell session
    sc
    ```

* **Edit Config Files:**
    ```bash
    # Open ~/.zshrc in the zed editor
    zshrc

    # Open ~/.config/starship.toml in zed
    sship

    # Open ~/.zsh/aliases.zsh in zed
    aliases
    ```
    *(Remember to change `zed` in the aliases if you use a different editor)*

### File Sharing Aliases

* **Share Current Directory via HTTP:**
    ```bash
    # Start a Python HTTP server on port 2121
    pshare

    # Start a Ruby HTTP server on port 2121
    rshare
    ```
    *Access files from another device on your network via `http://<your-ip>:2121`.*

### Integrated Tool Usage

* **Starship Prompt:**
    * The prompt itself is provided by Starship. It automatically shows context like current directory, Git branch status, Python version, etc. There's no command to run; it's always active.

* **Zoxide (Enhanced `cd`):**
    ```bash
    # After visiting /home/user/my/frequent/project a few times:
    # Jump directly to that directory from anywhere
    z project

    # See directories matching 'proj' and select interactively
    zi proj

    # Use tab completion to find matches
    z proj<TAB>
    ```

* **FZF (Fuzzy Finder):**
    * **History Search:** Press `Ctrl+R` and start typing to fuzzy-search your command history.
    * **File/Directory Completion:** Type a command and `**`, then press `Tab`. Example:
        ```bash
        vim **<TAB> # Opens fzf to select a file/dir recursively
        cd **<TAB>  # Opens fzf to select a directory to cd into
        ```
    * **Filtering:** Pipe output to `fzf` for interactive filtering.
        ```bash
        history | fzf
        ```

* **The Fuck (Command Correction):**
    ```bash
    # Make a typo
    git ststus
    # zsh: command not found: git

    # Type 'fuck' and press Enter
    fuck
    # It will suggest: git status [enter/↑/↓/ctrl+c]
    # Press Enter to execute the corrected command.
    ```

* **Pyenv (Python Version Management):**
    ```bash
    # List installed Python versions
    pyenv versions

    # List available versions to install
    pyenv install --list

    # Install a specific Python version
    pyenv install 3.11.5

    # Set the global default Python version
    pyenv global 3.11.5

    # Set a Python version for the current directory only
    pyenv local 3.10.1
    ```

* **Zsh Completion (Case-Insensitive & Menu):**
    * **Case-Insensitive:**
        ```bash
        # Type 'cd DOC' then press Tab
        # It will complete to 'cd Documents' (if that directory exists)
        cd DOC<TAB>
        ```
    * **Menu Selection:**
        ```bash
        # Type 'git ' then press Tab
        # A menu appears listing possible git commands. Use arrow keys or type further to select.
        git <TAB>
        ```
