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

1. Clone this repository to your home directory:

```bash
git clone <your-repo-url> ~/.dotfiles
cd ~/.dotfiles
```

2. Stow the packages you want:

```bash
# Install zsh configuration
stow zsh

# Install all packages at once
stow */
```

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

## Notes

- **Sensitive files**: This repo has a `.gitignore` to prevent committing sensitive files like API keys
- **Machine-specific configs**: Consider using separate branches or conditional logic for different machines
- Always backup your dotfiles before stowing for the first time
