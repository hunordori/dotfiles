# dotfiles

Personal dotfiles managed with [chezmoi](https://www.chezmoi.io/), supporting **macOS** and **Ubuntu**.

## What's included

| Category | Tool | Config |
|----------|------|--------|
| Shell | zsh + oh-my-zsh | `.zshrc`, `.zprofile`, `.bashrc` |
| Prompt | [Starship](https://starship.rs/) | `~/.config/starship.toml` |
| Terminal | [Ghostty](https://ghostty.org/) | macOS only |
| Navigation | [zoxide](https://github.com/ajeetdsouza/zoxide) | `z` command |
| Plugins | zsh-autosuggestions, zsh-syntax-highlighting, zsh-history-substring-search | via oh-my-zsh custom |

## Quick start

### New machine

```bash
sh -c "$(curl -fsSL https://get.chezmoi.io)" -- init --apply hunordori
```

### Existing chezmoi install

```bash
chezmoi init hunordori
chezmoi apply
```

## How it works

Chezmoi templates handle cross-platform differences automatically:

- **macOS** -- Homebrew paths, pnpm, chruby, Ghostty config
- **Linux** -- Standard `/usr/local` paths, apt-friendly tool installs

### run_once scripts

On first apply, chezmoi automatically:

1. Installs **oh-my-zsh** and custom plugins (autosuggestions, syntax-highlighting, history-substring-search)
2. Installs **starship** and **zoxide** (via Homebrew on macOS, curl installers on Linux)

## Structure

```
.
├── .chezmoi.toml.tmpl                  # Chezmoi config template
├── .chezmoiignore                      # Platform-specific ignores
├── dot_bashrc                          # Minimal bash config
├── dot_zprofile                        # Login shell env vars
├── dot_zshrc.tmpl                      # Zsh config (templated per OS)
├── dot_config/
│   └── starship.toml                   # Starship prompt theme
├── private_Library/.../ghostty/config  # Ghostty terminal config (macOS)
├── run_once_01-install-oh-my-zsh.sh.tmpl
├── run_once_02-install-tools.sh.tmpl
└── scripts/
    └── bootstrap.sh                    # Standalone bootstrap script
```

## Day-to-day usage

```bash
# Edit a managed file
chezmoi edit ~/.zshrc

# See what would change
chezmoi diff

# Apply changes
chezmoi apply

# Add a new file to chezmoi
chezmoi add ~/.config/some/config

# Pull latest and apply
chezmoi update
```

## Custom functions

- **`cpgh <server>`** -- Copies your local terminfo to a remote server via SSH (useful for Ghostty/modern terminal compatibility)

## Speed optimizations

- oh-my-zsh plugins are loaded selectively (no bloated defaults)
- nvm is lazy-loaded (only sources if `$NVM_DIR` exists)
- Starship prompt is fast by design (async git info)
- `run_once` scripts only execute on first apply, not every time
