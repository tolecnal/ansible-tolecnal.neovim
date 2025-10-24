# tolecnal.neovim Ansible Role - Usage Guide

## Quick Start

### 1. Install Role from Galaxy

\`\`\`bash
ansible-galaxy install tolecnal.neovim
\`\`\`

Or install with dependencies:
\`\`\`bash
ansible-galaxy install -r requirements.yml
\`\`\`

### 2. Run the Playbook

For local installation:
\`\`\`bash
ansible-playbook playbook.yml
\`\`\`

For remote hosts:
\`\`\`bash
ansible-playbook -i inventory playbook.yml
\`\`\`

With specific users:
\`\`\`bash
ansible-playbook playbook.yml -e "target_users=['alice','bob']"
\`\`\`

## Installation Methods

### Method 1: Local Installation (Default)

The provided `inventory` file is configured for local installation:

\`\`\`bash
ansible-playbook playbook.yml
\`\`\`

### Method 2: Remote Installation

Edit the `inventory` file:

\`\`\`ini
[development]
server1.example.com ansible_user=ubuntu
server2.example.com ansible_user=ubuntu

[development:vars]
ansible_python_interpreter=/usr/bin/python3
\`\`\`

Then run:
\`\`\`bash
ansible-playbook -i inventory playbook.yml --ask-become-pass
\`\`\`

### Method 3: Multiple Users

Create a custom playbook:

## \`\`\`yaml

- name: Install Neovim with LazyVim for multiple users
  hosts: development
  become: yes
  roles: - role: tolecnal.neovim
  vars:
  target_users: - user1 - user2 - user3
  \`\`\`

## Customization Examples

### Install Specific Neovim Version

## \`\`\`yaml

- hosts: development
  become: yes
  roles: - role: tolecnal.neovim
  vars:
  neovim_version: "v0.9.5" # or "stable" or "nightly"
  go_version: "1.21.5"
  nodejs_version: "20"
  \`\`\`

### Install Bob via Binary (Faster)

## \`\`\`yaml

- hosts: development
  become: yes
  roles: - role: tolecnal.neovim
  vars:
  bob_install_method: "binary"
  rust_install: false # Not needed for binary installation
  \`\`\`

### Minimal Installation

## \`\`\`yaml

- hosts: development
  become: yes
  roles: - role: tolecnal.neovim
  vars:
  install_lazygit: false
  install_bottom: false
  install_delta: false
  \`\`\`

### Use Custom LazyVim Configuration

## \`\`\`yaml

- hosts: development
  become: yes
  roles: - role: tolecnal.neovim
  vars:
  lazyvim_custom_config_url: "<https://github.com/yourusername/your-lazyvim-config.git>"
  \`\`\`

## Post-Installation

### First Launch

1. Open Neovim:
   \`\`\`bash
   nvim
   \`\`\`

2. LazyVim will automatically start installing plugins. You'll see a progress window.

3. Wait for all plugins to install (2-5 minutes typically).

4. Close and reopen Neovim:
   \`\`\`bash
   :qa
   nvim
   \`\`\`

### Verify Installation

\`\`\`bash

# Check Neovim version

nvim --version

# Check Bob installation

bob --version
bob list

# Check health

nvim +checkhealth +qall

# Check installed tools

rustc --version
go version
node --version
tree-sitter --version
lazygit --version
\`\`\`

### Configure LazyVim

LazyVim configuration is located at `~/.config/nvim/`:

\`\`\`
~/.config/nvim/
├── lua/
│ ├── config/
│ │ ├── autocmds.lua # Auto commands
│ │ ├── keymaps.lua # Key mappings
│ │ ├── lazy.lua # Plugin manager config
│ │ └── options.lua # Neovim options
│ └── plugins/
│ └── example.lua # Add your plugins here
├── init.lua # Entry point
└── stylua.toml # Lua formatter config
\`\`\`

### Add Custom Plugins

Create a new file in `~/.config/nvim/lua/plugins/`:

\`\`\`lua
-- ~/.config/nvim/lua/plugins/custom.lua
return {
{
"folke/zen-mode.nvim",
cmd = "ZenMode",
opts = {
window = {
width = 120,
},
},
},
}
\`\`\`

### Customize Keymaps

Edit `~/.config/nvim/lua/config/keymaps.lua`:

\`\`\`lua
-- Add your custom keymaps
local map = vim.keymap.set

map("n", "<leader>z", "<cmd>ZenMode<cr>", { desc = "Zen Mode" })
\`\`\`

## Managing Neovim Versions with Bob

Bob makes it incredibly easy to switch between Neovim versions:

### List Available Versions

\`\`\`bash

# Show all installed versions

bob list

# Show currently used version

bob list | grep ">"
\`\`\`

### Install Different Versions

\`\`\`bash

# Install latest stable

bob install stable

# Install nightly build

bob install nightly

# Install specific version

bob install v0.9.5

# Install latest version

bob install latest
\`\`\`

### Switch Between Versions

\`\`\`bash

# Use stable version

bob use stable

# Use nightly version

bob use nightly

# Use specific version

bob use v0.9.5
\`\`\`

### Update Versions

\`\`\`bash

# Update stable to latest stable release

bob update stable

# Update nightly to latest nightly build

bob update nightly
\`\`\`

### Remove Versions

\`\`\`bash

# Uninstall a specific version

bob uninstall v0.9.4

# Uninstall nightly

bob uninstall nightly
\`\`\`

### Rollback to Previous Version

\`\`\`bash

# If you have issues with a new version, simply switch back

bob list # See what versions you have
bob use v0.9.5 # Switch to a known good version
\`\`\`

## Troubleshooting

### Plugins Not Installing

\`\`\`bash

# Open Neovim and run

:Lazy sync
:Lazy clean
:Lazy update
\`\`\`

### LSP Not Working

\`\`\`bash

# Open Neovim and run

:Mason

# Install required LSP servers from the Mason UI

\`\`\`

### Tree-sitter Errors

\`\`\`bash

# Open Neovim and run

:TSUpdate
:TSInstall all
\`\`\`

### Clipboard Not Working

Make sure xclip is installed:
\`\`\`bash
sudo apt install xclip
\`\`\`

### Rust/Go Not in PATH

Source your shell configuration:
\`\`\`bash
source ~/.bashrc

# or

source ~/.zshrc
\`\`\`

### Bob Command Not Found

Source your shell configuration:
\`\`\`bash
source ~/.bashrc

# or

source ~/.zshrc
\`\`\`

Or add to PATH manually:
\`\`\`bash
export PATH="$HOME/.local/share/bob/nvim-bin:$PATH"
\`\`\`

### Neovim Not Found After Bob Installation

\`\`\`bash

# Ensure a version is installed and set as active

bob install stable
bob use stable

# Verify PATH includes Bob's nvim-bin directory

echo $PATH | grep bob
\`\`\`

## Updating

### Update Neovim

Using Bob (recommended):
\`\`\`bash

# Update to latest stable

bob update stable
bob use stable

# Or switch to nightly

bob install nightly
bob use nightly
\`\`\`

Or re-run the playbook with a new version:
\`\`\`bash
ansible-playbook playbook.yml -e "neovim_version=v0.10.0"
\`\`\`

### Update Bob

\`\`\`bash

# If installed via cargo

cargo install --git <https://github.com/MordechaiHadad/bob.git> --force

# If installed via binary, re-run the playbook

ansible-playbook playbook.yml
\`\`\`

## Uninstallation

To remove LazyVim (keeps Neovim and Bob installed):

\`\`\`bash
rm -rf ~/.config/nvim
rm -rf ~/.local/share/nvim
rm -rf ~/.local/state/nvim
rm -rf ~/.cache/nvim
\`\`\`

To remove Bob and all Neovim versions:

\`\`\`bash

# Remove Bob

rm -rf ~/.local/share/bob
rm /usr/local/bin/bob # If installed via binary

# Remove Bob from PATH (edit ~/.bashrc or ~/.zshrc)

# Remove the line: export PATH="$HOME/.local/share/bob/nvim-bin:$PATH"

\`\`\`

To restore backup:
\`\`\`bash
mv ~/.config/nvim.backup.TIMESTAMP ~/.config/nvim
\`\`\`

## Additional Resources

- [Bob Version Manager](https://github.com/MordechaiHadad/bob)
- [LazyVim Documentation](https://www.lazyvim.org/)
- [Neovim Documentation](https://neovim.io/doc/)
- [Lazy.nvim Plugin Manager](https://github.com/folke/lazy.nvim)
- [Mason LSP Installer](https://github.com/williamboman/mason.nvim)
