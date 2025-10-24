# Contributing to tolecnal.neovim

Thank you for your interest in contributing to the tolecnal.neovim Ansible role!

## Getting Started

1. Fork the repository
2. Clone your fork: `git clone https://github.com/yourusername/ansible-role-neovim.git`
3. Create a feature branch: `git checkout -b feature/your-feature-name`
4. Make your changes
5. Test your changes (see Testing Your Changes section below)
6. Commit your changes: `git commit -am 'Add some feature'`
7. Push to the branch: `git push origin feature/your-feature-name`
8. Create a Pull Request

## Development Setup

### Prerequisites

- Python 3.8+
- Docker (for Molecule testing)
- Ansible 2.9+

### Install Development Dependencies

\`\`\`bash

# Install Python dependencies

pip3 install -r molecule/requirements.txt

# Install Ansible Galaxy dependencies

ansible-galaxy install -r requirements.yml
\`\`\`

## Testing Your Changes

All contributions must pass the following tests:

### Running Linters

\`\`\`bash

# Ansible lint

ansible-lint

# YAML lint

yamllint .
\`\`\`

### Running Molecule Tests

\`\`\`bash

# Test all scenarios

molecule test --all

# Test specific scenario

molecule test --scenario-name default
\`\`\`

### Manual Testing Process

For manual testing on your local machine:

\`\`\`bash
ansible-playbook playbook.yml --check # Dry run
ansible-playbook playbook.yml # Actual run
\`\`\`

## Code Style

- Follow Ansible best practices
- Use meaningful variable names
- Add comments for complex logic
- Keep tasks idempotent
- Use YAML anchors to reduce duplication where appropriate

## Documentation

- Update README.md for new features or changed behavior
- Update USAGE.md with usage examples
- Add inline comments for complex tasks
- Update TESTING.md if adding new test scenarios

## Pull Request Guidelines

- Provide a clear description of the changes
- Reference any related issues
- Ensure all tests pass
- Update documentation as needed
- Keep commits atomic and well-described
- Squash commits before merging if requested

## Reporting Issues

When reporting issues, please include:

- Ansible version
- Operating system and version
- Role version or commit hash
- Full error output
- Steps to reproduce
- Expected vs actual behavior

## Feature Requests

Feature requests are welcome! Please:

- Check if the feature already exists
- Provide a clear use case
- Explain why it would be useful to others
- Consider submitting a PR if you can implement it

## Questions

For questions about using the role, please:

- Check the README.md and USAGE.md first
- Search existing issues
- Open a new issue with the "question" label

Thank you for contributing!
