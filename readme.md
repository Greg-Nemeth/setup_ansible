# Ansible Dev Machine Setup

This Ansible playbook automates the setup and configuration of a development machine. It installs essential tools and configures the environment for software development on a Debian/Ubuntu-based system.

## Features

- **Package Management**: Installs `nala` for a better `apt` experience.
- **Development Tools**:
  - **Docker**: Installs Docker Engine, configures the official repository, and adds the user to the docker group.
  - **Homebrew**: Installs the Homebrew package manager.
  - **SDKMAN!**: Installs SDKMAN! for managing Java/JVM versions.
- **System Configuration**: Sets up user credentials and ensures the system package cache is up to date.

## Prerequisites

- **Ansible**: Ensure Ansible is installed on your machine.
  ```bash
  sudo apt update
  sudo apt install ansible
  ```
- **Git**: Required to clone this repository.

## Repository Structure

```
.
├── inventory       # Defines the target hosts (configured for localhost)
├── playbook.yml    # Main playbook entry point
├── ansible.cfg     # Ansible configuration file
└── roles/          # Ansible roles for different components
    ├── common      # Basic system setup and user configuration
    ├── docker      # Docker installation and configuration
    ├── homebrew    # Homebrew installation
    ├── nala        # Nala installation
    └── sdkman      # SDKMAN! installation
```

## Configuration

The playbook uses the following variables defined in `playbook.yml`:

- `target_user`: The username to configure (default: `greg`).
- `user_password`: The password for the user, encrypted with Ansible Vault.

### Customizing the User

To run this playbook for a different user, you can override the `target_user` variable at runtime using the `-e` flag.

## Usage

1. **Clone the repository:**
   ```bash
   git clone <repository-url>
   cd setup_ansible
   ```

2. **Run the Playbook:**
   Since the `user_password` variable is encrypted with Ansible Vault, you must provide the vault password to run the playbook.

   **Option A: Interactive Password Prompt**
   ```bash
   ansible-playbook playbook.yml --ask-vault-pass
   ```

   **Option B: Using a Password File**
   If you have the vault password saved in a file (e.g., `vault_pass.txt`), you can use it to avoid interactive prompts:
   ```bash
   ansible-playbook playbook.yml --vault-password-file vault_pass.txt
   ```

   *Security Note: Never commit your vault password file to version control.*

3. **Overriding the User:**
   To setup a user other than the default `greg`:
   ```bash
   ansible-playbook playbook.yml -e "target_user=your_username" --ask-vault-pass
   ```

## Troubleshooting

- **Vault Errors**: If you see a "Decryption failed" error, ensure you are providing the correct Ansible Vault password.
- **Permission Errors**: The playbook uses `become: yes` to run tasks with sudo privileges. You may be prompted for your sudo password if passwordless sudo is not configured.
