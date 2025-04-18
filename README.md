# nix-zshrc
*nix zsh development setup

Install Starship first as per the website.

**Requirements**
- Starship (https://starship.rs)
- Homebrew (https://brew.sh)
- pyenv (Python Version Manager)
- nvm (Node Version Manager)
- bat (A cat(1) clone with wings)
- bpytop (Linux/OSX/FreeBSD resource monitor)
- eza: Modern ls replacement
- gdu: Disk usage analyzer
- git (GIT Version Control)
- htop (Interactive process viewer)
- rg (ripgrep): (Line-oriented search tool that recursively searches directories for a regex pattern)
- tree (Recursive directory listing command)
- nvim (Neovim /w lazyvim)
- wget2 (A modern successor to wget)

**Important Considerations:**

* **Package Managers:** The recommended installation method for most of these tools is via your system's package manager (e.g., `brew` on macOS, `apt` or `yum` on Linux). This helps manage dependencies and updates.
* **`nvm` and `pyenv`:** These tools primarily manage the environment for Node.js and Python, respectively. The installation of the tools below should generally not interfere with their operation. You'll typically install these tools system-wide or within your user's environment, separate from the versions managed by `nvm` and `pyenv`.

Below are instructions for installing each item. Choose the method that best suits your operating system.

## macOS
```bash
brew install bat bpytop eza gdu git htop ripgrep tree wget neovim pyenv
git clone --depth=1 [https://github.com/LazyVim/starter](https://github.com/LazyVim/starter) ~/.config/nvim && sh ~/.config/nvim/install.sh
```

## Linux (Debian/Ubuntu)
```bash
sudo apt update && sudo apt install bat bpytop eza git htop ripgrep tree wget2 neovim python3-pip -y
pip3 install bpytop # Install bpytop via pip for more recent versions
git clone --depth=1 [https://github.com/LazyVim/starter](https://github.com/LazyVim/starter) ~/.config/nvim && sh ~/.config/nvim/install.sh
curl [https://pyenv.run](https://pyenv.run) | bash
```

## Linux (Fedora/CentOS/RHEL)
```bash
sudo dnf install bat bpytop eza git htop ripgrep tree wget2 neovim python3-pip -y
# or sudo yum install bat bpytop eza git htop ripgrep tree wget2 neovim python3-pip -y
pip3 install bpytop # Install bpytop via pip for more recent versions
git clone --depth=1 [https://github.com/LazyVim/starter](https://github.com/LazyVim/starter) ~/.config/nvim && sh ~/.config/nvim/install.sh
curl [https://pyenv.run](https://pyenv.run) | bash
```

## After Installation for pyenv (if missing from .zshrc)
```bash
export PYENV_ROOT="$HOME/.pyenv"
command -v pyenv >/dev/null || export PATH="$PYENV_ROOT/bin:<span class="math-inline">PATH"
eval "</span>(pyenv init -)"
```