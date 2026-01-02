# Dotfiles

My personal dotfiles managed with [GNU Stow](https://www.gnu.org/software/stow/).

## Prerequisites

Install GNU Stow:

```bash
# macOS
brew install stow

# Ubuntu/Debian
sudo apt install stow

# Arch Linux
sudo pacman -S stow
```

## Installation

### Quick Install (All Packages)

```bash
# Clone with submodules
git clone --recurse-submodules git@github.com:arnold-lei/.dotfiles.git ~/.dotfiles
cd ~/.dotfiles

# Stow all packages
stow */
```

### Step-by-Step Install

1. Clone this repository to your home directory:

```bash
git clone git@github.com:arnold-lei/.dotfiles.git ~/.dotfiles
cd ~/.dotfiles
```

2. Initialize and update git submodules (required for nvim config):

```bash
git submodule update --init --recursive
```

3. Stow the packages you want:

```bash
# Install zsh configuration
stow zsh

# Install nvim configuration
stow nvim

# Install tmux configuration
stow tmux

# Install custom scripts
stow scripts

# Install all packages at once
stow */
```

### First Time Setup Notes

- **Backup existing configs**: Before stowing, backup any existing dotfiles
  ```bash
  mv ~/.zshrc ~/.zshrc.backup
  mv ~/.config/nvim ~/.config/nvim.backup
  mv ~/.tmux.conf ~/.tmux.conf.backup
  mv ~/.local/bin ~/.local/bin.backup  # If you have existing scripts
  ```

- **Handle conflicts**: If stow reports conflicts, move or backup the existing files first

## Usage

### Installing (Stowing) a Package

To create symlinks for a package:

```bash
cd ~/.dotfiles
stow <package-name>
```

Example:
```bash
stow zsh    # Creates ~/.zshrc -> ~/.dotfiles/zsh/.zshrc
```

### Removing (Unstowing) a Package

To remove symlinks for a package:

```bash
cd ~/.dotfiles
stow -D <package-name>
```

Example:
```bash
stow -D zsh    # Removes the ~/.zshrc symlink
```

### Restowing a Package

To refresh symlinks (useful after updating files):

```bash
cd ~/.dotfiles
stow -R <package-name>
```

### Dry Run

To see what stow would do without actually doing it:

```bash
stow -n <package-name>    # Dry run for stowing
stow -nD <package-name>   # Dry run for unstowing
```

## Structure

Each directory represents a "package" and mirrors your home directory structure:

```
~/.dotfiles/
├── zsh/
│   └── .zshrc          # Will be symlinked to ~/.zshrc
└── vim/
    └── .vimrc          # Will be symlinked to ~/.vimrc
```

For nested configs like `~/.config/nvim/init.vim`:

```
~/.dotfiles/
└── nvim/
    └── .config/
        └── nvim/
            └── init.vim    # Will be symlinked to ~/.config/nvim/init.vim
```

## Packages

- **zsh**: Zsh configuration with oh-my-zsh and powerlevel10k
- **nvim**: Neovim configuration (git submodule from [kickstart.nvim](https://github.com/arnold-lei/kickstart.nvim))
- **tmux**: Tmux configuration with custom keybindings and layouts
- **scripts**: Custom shell scripts in `~/.local/bin`
  - `open-git.sh`: Opens the current git repository in the browser
  - `tmux-sessionizer`: Quick tmux session switcher with fzf

## Adding New Dotfiles

1. Create a package directory (e.g., `vim`)
2. Move your dotfile into the package directory (e.g., `mv ~/.vimrc vim/.vimrc`)
3. Run stow: `stow vim`
4. Commit to git: `git add vim/ && git commit -m "Add vim config"`

## Troubleshooting

### Conflict: File already exists

If stow complains about existing files:

```bash
# Option 1: Backup and remove the existing file
mv ~/.zshrc ~/.zshrc.backup
stow zsh

# Option 2: Use --adopt to adopt existing files into your dotfiles
stow --adopt zsh
```

### Wrong symlink target

If the symlink points to the wrong place:

```bash
# Restow the package
stow -R <package-name>
```

## Working with Git Submodules

This repository uses git submodules for some configurations (like nvim). Here are common operations:

### Updating Submodules

To pull the latest changes from all submodules:

```bash
git submodule update --remote --merge
```

To update a specific submodule:

```bash
cd nvim/.config/nvim
git pull origin main
cd ../../..
git add nvim/.config/nvim
git commit -m "Update nvim submodule"
```

### After Cloning on a New Machine

If you cloned without `--recurse-submodules`:

```bash
cd ~/.dotfiles
git submodule update --init --recursive
```

## Notes

- **Sensitive files**: This repo has a `.gitignore` to prevent committing sensitive files like API keys
- **Machine-specific configs**: Consider using separate branches or conditional logic for different machines
- **Submodules**: The nvim config is maintained as a separate repository and included as a submodule
- Always backup your dotfiles before stowing for the first time
