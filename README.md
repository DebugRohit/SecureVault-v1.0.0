# SecureVault

SecureVault is a command-line password manager built with Python that securely stores credentials using encrypted local storage and master-password authentication.

The project focuses on practical security, clean software architecture, and maintainable Python development. It demonstrates encryption, authentication, password management, automated testing, and secure handling of sensitive runtime data.

---

## Features

- Master password setup and authentication
- Encrypted credential storage using Fernet encryption
- Secure password generation
- Password strength analysis and feedback
- Add, view, update, search, and delete credentials
- Password reveal only after successful authentication
- Vault backup and restore functionality
- Modular project architecture
- Automated testing with Pytest
- GitHub Actions continuous integration
- Static analysis using Flake8
- Code formatting with Black

---

## Technology Stack

| Technology | Purpose |
|------------|---------|
| Python | Core application |
| Cryptography (Fernet) | Credential encryption |
| Hashlib | Master password hashing |
| Secrets | Secure password generation |
| JSON | Local credential storage |
| Pytest | Automated testing |
| Black | Code formatting |
| Flake8 | Static code analysis |
| GitHub Actions | Continuous Integration |

---

## Project Structure

```text
SecureVault/
│
├── .github/
│   └── workflows/
├── src/
│   ├── auth/
│   ├── crypto/
│   ├── exceptions/
│   ├── models/
│   ├── services/
│   ├── utils/
│   └── main.py
├── tests/
├── .gitignore
├── LICENSE
├── pyproject.toml
├── requirements.txt
└── README.md
```

---

## Security Highlights

SecureVault was designed with security as a primary objective.

- Credentials are encrypted before being written to disk.
- Master passwords are securely hashed and never stored as plaintext.
- Passwords remain hidden during normal credential listing.
- Stored passwords are revealed only after successful authentication.
- Encryption keys, vault data, backups, and runtime files are intentionally excluded from version control.

---

## Getting Started

### Clone the repository

```bash
git clone https://github.com/DebugRohit/SecureVault-v1.0.0.git
cd SecureVault-v1.0.0
```

### Create a virtual environment

```bash
python -m venv venv
```

### Activate the environment

**Windows (PowerShell)**

```powershell
.\venv\Scripts\Activate.ps1
```

**Linux / macOS**

```bash
source venv/bin/activate
```

### Install dependencies

```bash
pip install -r requirements.txt
```

---

## Run the Application

```bash
python -m src.main
```

On the first launch, SecureVault creates the required local security files and guides you through master password setup.

---

## Running Tests

Execute the complete test suite:

```bash
pytest
```

Project status:

- ✔ Automated unit tests
- ✔ Encryption workflow verified
- ✔ Authentication workflow verified
- ✔ Backup & Restore verified
- ✔ Password generation verified
- ✔ Password strength analysis verified

---

## Why This Project?

SecureVault was developed to demonstrate practical Python software engineering through a real-world security application.

Key areas demonstrated include:

- Secure authentication
- Symmetric encryption
- Password management
- Modular application design
- Exception handling
- Automated testing
- Code quality tooling
- Git-based version control
- Continuous integration

---

## Future Improvements

Potential enhancements include:

- Multi-user support
- Password import/export
- Search filters
- Secure clipboard integration
- Graphical user interface (GUI)
- Cross-platform packaging

---

## License

This project is licensed under the MIT License.

See the **LICENSE** file for details.

---

## Integrated by :

`Rohit Pujari`

`Email: rohit.pujari@icloud.com`

`Thank You`
