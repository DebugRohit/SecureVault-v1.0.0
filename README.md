# SecureVault

SecureVault is a command-line password manager written in Python. It provides encrypted local credential storage, master-password authentication, secure password generation, password-strength feedback, and vault backup and restoration.

The project is designed as a local-first password management application. Sensitive runtime files remain on the local machine and are excluded from Git tracking.

## Features

* Master-password setup and authentication
* Secure master-password hashing
* Encrypted credential storage using Fernet symmetric encryption
* Add credentials with manually entered passwords
* Generate secure passwords automatically
* Password-strength analysis and feedback
* List stored credentials without exposing passwords
* Search credentials by service name
* Update existing credentials
* Delete stored credentials
* Intentionally reveal a stored password after authentication
* Create vault backups
* Restore vault data from backups
* Application logging
* Custom exception handling
* Automated testing with pytest
* Code formatting with Black
* Static code checks with Flake8
* Dependency vulnerability auditing with pip-audit
* Continuous integration with GitHub Actions
* Protection of sensitive runtime files through `.gitignore`

## Security Design

SecureVault separates authentication, encryption, vault management, password utilities, backup operations, and application logging into dedicated modules.

The application is intended to keep credential data local to the machine on which it is used.

### Master Password Authentication

SecureVault requires a master password before protected vault operations can be performed.

During initial setup, the application creates the required master-password authentication data. The original master password is not stored as readable plaintext.

Subsequent application sessions require successful master-password verification before access to the vault is granted.

### Credential Encryption

Credential passwords are encrypted before they are written to the vault.

SecureVault uses Fernet symmetric encryption from the Python `cryptography` package. Encrypted values are stored in the vault instead of readable credential passwords.

The encryption key is maintained as a local runtime file and is excluded from Git tracking.

### Password Display

Stored passwords are not displayed during normal credential listing.

The list operation shows credential information without automatically exposing the stored password.

A password is decrypted and displayed only when the user intentionally selects the password-reveal operation and successfully completes the required authentication flow.

Generated passwords are also not automatically printed to the terminal. The application confirms successful generation without exposing the generated value unnecessarily.

### Sensitive Runtime Files

SecureVault generates local files that contain security-related application data.

These files may include:

```text
data/vault.json
data/master.hash
data/secret.key
```

Depending on the application operation, backup and log files may also be generated.

Sensitive runtime files are excluded from Git tracking through `.gitignore`.

These files should not be committed to a repository, shared publicly, or manually modified.

## Project Structure

```text
SecureVault/
|
|-- .github/
|   `-- workflows/
|       `-- python.yml
|
|-- data/
|   |-- vault.json
|   |-- master.hash
|   `-- secret.key
|
|-- src/
|   |-- auth/
|   |   `-- authentication.py
|   |
|   |-- crypto/
|   |   `-- encryption.py
|   |
|   |-- exceptions/
|   |   `-- custom_exceptions.py
|   |
|   |-- models/
|   |   `-- vault.py
|   |
|   |-- services/
|   |   |-- backup_service.py
|   |   |-- password_generator.py
|   |   `-- password_strength.py
|   |
|   |-- utils/
|   |   |-- helper.py
|   |   `-- logger.py
|   |
|   `-- main.py
|
|-- tests/
|
|-- .gitignore
|-- README.md
`-- requirements.txt
```

The `data` directory contains local runtime security files. These files are not intended to be tracked by Git.

## Requirements

SecureVault requires:

* Python
* pip
* Git

A Python virtual environment is recommended.

To confirm that Python is installed, run:

```bash
python --version
```

To confirm that Git is installed, run:

```bash
git --version
```

## Installation

### 1. Clone the Repository

Clone the SecureVault repository:

```bash
git clone <repository-url>
```

Replace `<repository-url>` with the URL of the SecureVault GitHub repository.

Move into the project directory:

```bash
cd SecureVault
```

### 2. Create a Virtual Environment

Create a Python virtual environment:

```bash
python -m venv venv
```

### 3. Activate the Virtual Environment

On Windows PowerShell:

```powershell
.\venv\Scripts\Activate.ps1
```

On Windows Command Prompt:

```cmd
venv\Scripts\activate
```

On Linux or macOS:

```bash
source venv/bin/activate
```

After activation, the terminal should display the virtual environment name:

```text
(venv)
```

### 4. Upgrade pip

Upgrade pip inside the virtual environment:

```bash
python -m pip install --upgrade pip
```

### 5. Install Dependencies

Install the project dependencies:

```bash
python -m pip install -r requirements.txt
```

### 6. Verify the Installation

Run the automated test suite:

```bash
python -m pytest -v
```

The project is ready to run when the test suite completes successfully.

## Running SecureVault

Activate the virtual environment and run the application from the project root directory:

```bash
python -m src.main
```

SecureVault starts in the terminal and presents the available command-line operations.

## First-Time Setup

When SecureVault runs for the first time, the required runtime security files may not exist.

The application initializes the required local data and prompts for master-password setup.

The master password is used to authenticate access to protected vault operations.

The master password should be remembered and kept private. SecureVault does not store the original master password as readable plaintext.

The following runtime files may be created:

```text
data/master.hash
data/secret.key
data/vault.json
```

These files are local application data and are excluded from Git tracking.

Do not delete or manually modify the encryption key if the vault contains credentials. Encrypted credential data may become inaccessible if the corresponding key is lost.

## Usage

SecureVault is operated through the command-line interface.

Start the application with:

```bash
python -m src.main
```

Authenticate with the configured master password when prompted.

### Adding a Credential

Use the add-credential operation to create a new vault entry.

The application accepts credential information such as:

```text
Service
Username
Password
```

A password can be entered manually or created through the password-generation functionality.

The credential password is encrypted before the vault data is saved.

### Generating a Password

SecureVault includes a secure password generator.

The generator uses Python security-focused random generation functionality to create passwords.

Generated passwords are not automatically exposed in routine terminal output. The application reports successful generation while avoiding unnecessary password display.

### Checking Password Strength

The password-strength service evaluates a password and provides strength-related feedback.

Password analysis can help identify passwords that are too weak or predictable.

A stronger password should generally be long, unique, and difficult to guess.

Passwords should not be reused across unrelated services.

### Listing Credentials

The credential-listing operation displays stored vault entries.

Passwords are not displayed as part of normal listing output.

This behavior reduces accidental credential exposure during terminal use, screen sharing, or terminal recording.

### Searching Credentials

The search operation can be used to locate stored credentials by service name.

Search results provide matching credential information without automatically revealing stored passwords.

### Updating a Credential

Use the update operation to modify an existing vault entry.

The application updates the selected credential and saves the resulting vault data.

Updated password values are protected before being written to storage.

### Deleting a Credential

Use the delete operation to remove a stored credential.

The selected entry should be reviewed before deletion is confirmed.

Deleted credentials may not be recoverable unless suitable backup data is available.

### Revealing a Stored Password

SecureVault provides an intentional password-reveal operation.

The reveal flow is separate from normal credential listing.

A stored password is decrypted and displayed only as part of the explicit reveal operation after the required authentication checks have succeeded.

Password reveal operations should be performed in a private terminal session.

Avoid revealing passwords while:

* Sharing the screen
* Recording the terminal
* Using a publicly visible computer
* Allowing another person to view the terminal

## Backup and Restore

SecureVault provides vault backup and restoration functionality.

### Creating a Backup

Use the backup operation to create a backup of the vault data.

Backup files should be treated as sensitive application data.

Do not commit backup files to Git or upload them to a public repository.

### Restoring a Backup

Use the restore operation to restore vault data from an available backup.

A restore operation may replace the current vault data.

Before restoring a backup:

1. Confirm that the intended backup file has been selected.
2. Verify that the backup belongs to the correct SecureVault environment.
3. Avoid manually modifying the backup file.
4. Preserve the current vault data when a separate backup is required.

The application handles restoration through the backup service.

## Module Overview

### `src/main.py`

The main application entry point.

It coordinates the command-line workflow and connects the authentication, vault, password, backup, and utility components.

### `src/auth/authentication.py`

Contains master-password setup and authentication functionality.

The module is responsible for protecting and verifying master-password authentication data.

### `src/crypto/encryption.py`

Contains credential encryption and decryption functionality.

The module manages the encryption operations used to protect credential passwords.

### `src/models/vault.py`

Contains vault-related data management and credential operations.

The vault module is responsible for storing and managing credential entries.

### `src/services/password_generator.py`

Contains secure password-generation functionality.

The service creates passwords using security-focused random generation.

### `src/services/password_strength.py`

Contains password-strength evaluation functionality.

The service analyses password characteristics and returns strength-related feedback.

### `src/services/backup_service.py`

Contains vault backup and restoration functionality.

The service manages the creation and restoration of vault backup data.

### `src/utils/helper.py`

Contains reusable helper functionality used by the application.

Shared utility logic is kept separate from the primary application workflow.

### `src/utils/logger.py`

Contains application logging configuration and related functionality.

Logging is intended for application events and diagnostic information.

Credential passwords, encryption keys, and other secrets should not be intentionally written to logs.

### `src/exceptions/custom_exceptions.py`

Contains application-specific exception classes.

Custom exceptions allow SecureVault to represent project-specific failure conditions more clearly.

## Testing

SecureVault uses `pytest` for automated testing.

Run the complete test suite:

```bash
python -m pytest -v
```

Run a specific test file:

```bash
python -m pytest tests/test_main.py -v
```

Run the test suite with concise output:

```bash
python -m pytest -q
```

Tests should be run before committing significant code changes.

## Code Formatting

SecureVault uses Black for Python code formatting.

Check formatting without modifying files:

```bash
python -m black --check .
```

Format the project:

```bash
python -m black .
```

After formatting, run the test suite again:

```bash
python -m pytest -v
```

## Static Code Checks

SecureVault uses Flake8 for static code checks.

Run:

```bash
python -m flake8 src tests
```

Flake8 helps identify style issues and common Python code problems.

## Dependency Security Audit

SecureVault uses `pip-audit` to check installed Python dependencies for known vulnerabilities.

Run:

```bash
python -m pip_audit
```

Dependency auditing should be performed after dependency changes and before important project releases.

## Continuous Integration

The project uses GitHub Actions for continuous integration.

The workflow configuration is stored in:

```text
.github/workflows/python.yml
```

The continuous integration workflow automatically validates project changes in the configured GitHub Actions environment.

Automated validation helps identify test or code-quality failures before changes are accepted into the project workflow.

## Git Safety

Sensitive runtime files must not be committed to Git.

Before creating a commit, run:

```bash
git status
```

Review every displayed file.

Files containing vault data, master-password authentication data, encryption keys, backups, or sensitive runtime information should not be tracked.

To check whether important runtime files are tracked, run:

```bash
git ls-files data/vault.json
git ls-files data/master.hash
git ls-files data/secret.key
```

When these files are correctly untracked, the commands should produce no file output.

The `.gitignore` file should continue to protect generated security-related runtime data.

## Development Checks

Before committing project changes, run the following checks:

```bash
python -m pytest -v
python -m black --check .
python -m flake8 src tests
python -m pip_audit
git status
```

Review the output of every command before committing.

The expected development state is:

* Automated tests pass
* Black reports no formatting changes are required
* Flake8 completes without unresolved issues
* Dependency auditing reports no known vulnerabilities requiring action
* Sensitive runtime files are not tracked
* The Git working tree contains only intended project changes

## Security Considerations

SecureVault is a local command-line password management project.

The following security practices should be maintained:

* Keep the master password private.
* Do not store the master password in source code.
* Do not commit the encryption key.
* Do not commit vault data.
* Do not commit master-password authentication data.
* Do not publish vault backups.
* Do not intentionally write passwords to application logs.
* Do not expose generated passwords through unnecessary terminal output.
* Reveal stored passwords only when required.
* Keep project dependencies updated.
* Audit dependencies regularly.
* Review Git changes before every commit.
* Protect the computer on which SecureVault is used.

The security of local runtime data also depends on the security of the operating system and user account running the application.

## Technology Stack

SecureVault is built with:

* Python
* `cryptography`
* `hashlib`
* `secrets`
* `json`
* `getpass`
* `pathlib`
* `logging`
* `argparse`
* `pytest`
* Black
* Flake8
* pip-audit
* Git
* GitHub Actions

## Project Status

The current SecureVault implementation includes:

* Master-password authentication
* Protected master-password data
* Encrypted credential storage
* Credential creation
* Secure password generation
* Password-strength analysis
* Credential listing without automatic password exposure
* Credential search
* Credential updates
* Credential deletion
* Intentional authenticated password reveal
* Vault backup and restoration
* Application logging
* Custom exceptions
* Automated tests
* Code-format validation
* Static code checks
* Dependency security auditing
* Continuous integration
* Sensitive runtime file protection

The core command-line application is implemented and tested.

## Purpose

SecureVault was developed as an advanced Python project focused on practical application structure and security-conscious development.

The project demonstrates the use of:

* Modular Python application design
* Authentication workflows
* Password hashing
* Symmetric encryption
* Local data persistence
* Secure random generation
* Command-line application development
* Exception handling
* Logging
* Automated testing
* Code-quality checks
* Dependency auditing
* Git version control
* Continuous integration

The project is intended for educational and portfolio use.

## Contributing

Contributions and improvements can be submitted through the standard Git workflow.

1. Fork the repository.
2. Create a development branch.
3. Make the required changes.
4. Run the project checks.
5. Review Git status.
6. Commit the intended changes.
7. Push the branch.
8. Open a pull request.

Before submitting changes, run:

```bash
python -m pytest -v
python -m black --check .
python -m flake8 src tests
python -m pip_audit
git status
```

Do not include credential data, master-password authentication files, encryption keys, private backups, or other sensitive runtime files in a contribution.

## License

This project is intended for educational and portfolio purposes.

Refer to the repository license file for the applicable usage and distribution terms.

## Author

Rohith Pujari

SecureVault was developed as a Python password management project focused on modular application design, encrypted local storage, authentication, testing, and security-conscious development.
