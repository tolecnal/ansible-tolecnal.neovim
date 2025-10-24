# Ansible Role: tolecnal.neovim

An Ansible role to install and configure Neovim with LazyVim on Ubuntu 24.x+, Debian 12+, and macOS.

## Description

This role installs all necessary dependencies and tools required to run Neovim with the LazyVim distribution, including:

- **Core Dependencies**: build tools, git, curl, wget, unzip, tar
- **Python**: python3, pip, pynvim
- **Search Tools**: ripgrep, fd, fzf
- **Programming Languages**: Rust, Go, Node.js
- **Build Tools**: cmake, ninja, pkg-config, libtool
- **Bob**: Neovim version manager for flexible version switching
- **Neovim**: Installed via Bob (latest stable or specified version)
- **Tree-sitter CLI**: For syntax highlighting
- **Optional Tools**: lazygit, bottom, delta

## Requirements

- Ansible 2.9 or higher
- **Linux**: Ubuntu 24.04+ or Debian 12+
- **macOS**: macOS 11+ with Homebrew installed
- Internet connection for downloading packages and tools

## Role Dependencies

This role depends on:

- `geerlingguy.nodejs` - For Node.js installation

Install dependencies with:
\`\`\`bash
ansible-galaxy install -r requirements.yml
\`\`\`

Or install this role directly from Galaxy:
\`\`\`bash
ansible-galaxy install tolecnal.neovim
\`\`\`

## Role Variables

Available variables are listed below, along with default values (see `defaults/main.yml`):

\`\`\`yaml

# Bob version manager for Neovim

bob_install_method: "cargo" # or "binary"
neovim_version: "stable" # or "nightly" or specific version like "v0.9.5"

# Language installations

rust_install: true # Required for Bob if using cargo method
go_install: true
go_version: "1.21.5"
nodejs_install: true
nodejs_version: "20"

# Tree-sitter

tree_sitter_install: true

# Target users for user-specific installations

target_users:

- "{{ ansible_user_id }}" # Default to current user

# LazyVim configuration

lazyvim_install: true
lazyvim_custom_config_url: "" # Set to your custom config git URL

# Optional tools

install_lazygit: true
install_bottom: true
install_delta: true
\`\`\`

## Example Playbook

### Basic installation (current user)

## \`\`\`yaml

- hosts: development
  become: yes
  roles: - role: tolecnal.neovim
  vars:
  neovim_version: "stable"
  go_version: "1.21.5"
  nodejs_version: "20"
  \`\`\`

### Install for multiple users

## \`\`\`yaml

- hosts: development
  become: yes
  roles: - role: tolecnal.neovim
  vars:
  target_users: - alice - bob - charlie
  neovim_version: "stable"
  \`\`\`

### Use nightly Neovim build

## \`\`\`yaml

- hosts: development
  become: yes
  roles: - role: tolecnal.neovim
  vars:
  neovim_version: "nightly"
  \`\`\`

### Use custom LazyVim configuration

## \`\`\`yaml

- hosts: development
  become: yes
  roles: - role: tolecnal.neovim
  vars:
  lazyvim_custom_config_url: "<https://github.com/yourusername/your-lazyvim-config.git>"
  \`\`\`

### Install Bob via binary (faster, no Rust compilation)

## \`\`\`yaml

- hosts: development
  become: yes
  roles: - role: tolecnal.neovim
  vars:
  bob_install_method: "binary"
  rust_install: false # Not needed for binary installation
  \`\`\`

### Minimal installation (no optional tools)

## \`\`\`yaml

- hosts: development
  become: yes
  roles: - role: tolecnal.neovim
  vars:
  install_lazygit: false
  install_bottom: false
  install_delta: false
  \`\`\`

### macOS-specific playbook

## \`\`\`yaml

- hosts: macos_hosts
  roles: - role: tolecnal.neovim
  vars:
  neovim_version: "stable"
  target_users: - "{{ ansible_user_id }}" # Homebrew will be used for package installation
  \`\`\`

## Usage

After running this role:

1. Launch Neovim with `nvim`
2. LazyVim will automatically install all plugins on first launch
3. Wait for the installation to complete (may take a few minutes)
4. Restart Neovim

### Managing Neovim Versions with Bob

Bob makes it easy to switch between Neovim versions:

\`\`\`bash

# List available versions

bob list

# Install a specific version

bob install v0.9.5

# Install nightly

bob install nightly

# Switch to a version

bob use v0.9.5

# Update to latest stable

bob update stable

# Uninstall a version

bob uninstall v0.9.4
\`\`\`

### Multi-User Installation

The role supports installing for multiple users simultaneously:

\`\`\`yaml
target_users:

- alice
- bob
- charlie
  \`\`\`

Each user will get:

- Bob installed in their home directory (`~/.local/share/bob`)
- Neovim accessible via Bob
- LazyVim configuration in `~/.config/nvim`
- Rust, Go, and Node.js PATH configurations in their shell profiles

### Using Custom LazyVim Configuration

You can use your own LazyVim configuration instead of the official starter:

1. Set `lazyvim_custom_config_url` to your git repository URL
2. The role will clone your custom config instead of the official starter
3. Your custom config should be a complete LazyVim configuration

Example:
\`\`\`yaml
lazyvim_custom_config_url: "<https://github.com/yourusername/my-lazyvim-config.git>"
\`\`\`

### Customizing LazyVim

- **Configuration**: Edit files in `~/.config/nvim/lua/config/`
- **Add Plugins**: Create files in `~/.config/nvim/lua/plugins/`
- **Keymaps**: Edit `~/.config/nvim/lua/config/keymaps.lua`

### Useful Commands

- `:Lazy` - Open plugin manager
- `:Mason` - Open LSP/DAP/Linter installer
- `:checkhealth` - Check Neovim health
- `<leader>` is mapped to `Space` by default

## What Gets Installed

### Linux (APT Packages)

- build-essential, git, curl, wget, unzip, tar
- python3, python3-pip, python3-venv
- fd-find, fzf, ripgrep
- xclip (clipboard support)
- cmake, ninja-build, pkg-config, libtool, autoconf, automake
- luarocks, gettext

### macOS (Homebrew Packages)

- git, curl, wget, unzip
- python3
- fd, fzf, ripgrep
- cmake, ninja, pkg-config, libtool, autoconf, automake
- luarocks, gettext, gcc

### Programming Languages

- **Rust**: Installed via rustup (stable toolchain) - required for Bob if using cargo method
- **Go**: Installed from official binaries (supports both Linux and macOS)
- **Node.js**: Installed via geerlingguy.nodejs role (cross-platform)

### Tools (Per-User)

- **Bob**: Neovim version manager for easy version switching (cross-platform)
- **Neovim**: Installed via Bob (latest stable or specified version)
- **tree-sitter-cli**: For syntax highlighting
- **lazygit**: TUI for git (optional, cross-platform)
- **bottom**: Modern system monitor (optional, cross-platform)
- **delta**: Better git diff viewer (optional, cross-platform)

## Supported Platforms

### Linux

- Ubuntu 24.04 (Noble)
- Ubuntu 23.10 (Mantic)
- Debian 12 (Bookworm)
- Debian 13 (Trixie)

### macOS

- macOS 11 (Big Sur) and later
- Both Intel (x86_64) and Apple Silicon (arm64) architectures
- **Prerequisite**: Homebrew must be installed

## Notes

- **Multi-user support**: Install for multiple users by setting `target_users` list
- **macOS**: Homebrew must be installed before running this role
- First Neovim launch will take several minutes to install plugins
- Existing Neovim configurations are backed up with timestamp
- Bob and language binaries are added to PATH in both `.bashrc` and `.zshrc`
- **Linux**: The `fd` command is symlinked from `fdfind` for compatibility
- **macOS**: The `fd` package is installed directly via Homebrew
- Bob stores Neovim versions in `~/.local/share/bob/` per user
- Custom LazyVim configs replace the official starter completely
- All optional tools (lazygit, bottom, delta) support both Linux and macOS

## Troubleshooting

### Check Neovim health

\`\`\`bash
nvim +checkhealth
\`\`\`

### Verify installations

\`\`\`bash
nvim --version
bob list
rustc --version
go version
node --version
tree-sitter --version
\`\`\`

### Plugin issues

\`\`\`bash

# Open Neovim and run

:Lazy sync
:Lazy clean
:Lazy update
\`\`\`

### Bob issues

\`\`\`bash

# Check Bob installation

bob --version

# Reinstall current version

bob install {{ neovim_version }}
bob use {{ neovim_version }}
\`\`\`

### macOS-specific issues

**Homebrew not found:**
\`\`\`bash

# Install Homebrew first

/bin/bash -c "$(curl -fsSL <https://raw.githubusercontent.com/Homebrew/install/HEAD/install.sh>)"
\`\`\`

**Permission issues:**

- Most tasks on macOS don't require `become: yes`
- Only system-wide installations need elevated privileges

## License

MIT

## Author Information

This role was created by tolecnal for setting up modern Neovim development environments with LazyVim and Bob version manager across Linux and macOS platforms with multi-user support.

For issues, feature requests, or contributions, please visit the GitHub repository.
