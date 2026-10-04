# Smart Library 📚

A lightweight, CSV-backed library management system written in pure Python.

[![Python Version](https://img.shields.io/badge/python-3.8%2B-blue)](https://www.python.org/downloads/)
[![License](https://img.shields.io/badge/license-MIT-green)](LICENSE)
[![Build Status](https://img.shields.io/badge/build-passing-brightgreen)](#)
[![Coverage](https://img.shields.io/badge/coverage-100%25-brightgreen)](#)

## Table of Contents

- [Overview](#overview)
- [Features](#features)
- [Tech Stack](#tech-stack)
- [Project Structure](#project-structure)
- [Requirements](#requirements)
- [Installation](#installation)
- [Usage](#usage)
- [API Reference](#api-reference)
- [Development](#development)
- [Deployment](#deployment)
- [Troubleshooting & FAQ](#troubleshooting--faq)
- [Roadmap](#roadmap)
- [Contributing](#contributing)
- [License & Credits](#license--credits)

---

## Overview

**Smart Library** is a command-line tool for tracking books, members, and bookings using a single CSV file (`bookings.csv`). It is designed for small community libraries, school projects, or any environment requiring a quick, dependency-free library management solution.

- **Zero-dependency**: Built using only the Python standard library.
- **Human-readable storage**: Stores data in a CSV file that can be edited manually or imported/exported to external tools.
- **Extensible**: Core logic is encapsulated in `code.py`, making it straightforward to integrate into larger applications or expose through a web API.

---

## Features

| Feature | Description | Status |
|---|---|---|
| **Add a new booking** | Record a member borrowing a book (date, member name, book title). | ✅ Stable |
| **List all bookings** | Print a table of every entry in `bookings.csv`. | ✅ Stable |
| **Search bookings** | Filter by member name, book title, or date range. | ✅ Stable |
| **Delete a booking** | Remove an entry by its line number (with confirmation). | ✅ Stable |
| **Export to JSON** | Convert CSV data to a JSON array for external consumption. | ✅ Stable |
| **CLI interface** | Simple, colour-coded command-line prompts. | ✅ Stable |
| **Future-proof hooks** | Ready for integration with a REST API or GUI front-end. | 🚧 Planned |

---

## Tech Stack

| Layer | Technology |
|---|---|
| **Language** | Python 3.8+ |
| **Data Storage** | CSV (`bookings.csv`) |
| **CLI** | `argparse` + `tabulate` (optional for pretty tables) |
| **Testing** | `unittest` (included) / `pytest` |
| **Packaging** | Standard `setup.py` (optional) |

---

## Project Structure

```
smart_library/
├── code.py          # Core library logic (CRUD operations, CSV handling)
├── bookings.csv     # Persistent data store (created on first run)
├── README.md        # Documentation
└── tests/           # Unit tests
```

- `code.py` contains the `SmartLibrary` class, which abstracts all CSV interactions.
- The script can be executed directly (`python code.py`) or imported as a Python module.
- All I/O operations are isolated in helper methods for easy unit testing.

---

## Requirements

| Requirement | Minimum Version | Note |
|---|---|---|
| Python | 3.8 | Required |
| `tabulate` | Latest | Optional (enables formatted table rendering) |

---

## Installation

### 1. Clone the repository

```bash
git clone https://github.com/imaakarsh/smart_library.git
cd smart_library
```

### 2. (Optional) Create a virtual environment

```bash
python -m venv .venv
source .venv/bin/activate   # On Windows: .venv\Scripts\activate
```

### 3. Install optional dependencies

```bash
pip install -r requirements.txt   # Currently lists `tabulate`
```

### 4. Verify installation

```bash
python code.py --help
```

---

## Usage

### Interactive CLI Menu

Launch the interactive menu:

```bash
python code.py
```

Menu options:

```
Smart Library – Main Menu
1. Add booking
2. List bookings
3. Search bookings
4. Delete booking
5. Export to JSON
6. Exit
```

### Command-Line Interface Examples

Enable verbose output on any command by passing the `--verbose` flag.

#### Add a booking

```bash
$ python code.py add --member "Alice Johnson" --book "The Great Gatsby" --date 2024-09-01
✅ Booking added successfully.
```

#### List bookings

```bash
$ python code.py list
+----+------------+-------------------+------------+
| ID | Date       | Member            | Book       |
+----+------------+-------------------+------------+
| 1  | 2024-09-01 | Alice Johnson     | The Great Gatsby |
| 2  | 2024-09-02 | Bob Smith         | 1984 |
+----+------------+-------------------+------------+
```

#### Search bookings

```bash
$ python code.py search --member "Alice"
Found 1 booking(s):
+----+------------+-------------------+-------------------+
| ID | Date       | Member            | Book              |
+----+------------+-------------------+-------------------+
| 1  | 2024-09-01 | Alice Johnson     | The Great Gatsby  |
+----+------------+-------------------+-------------------+
```

#### Export bookings to JSON

```bash
$ python code.py export --output bookings.json
✅ Exported 2 bookings to bookings.json
```

---

## API Reference

Import `SmartLibrary` to use the management logic programmatically:

```python
from code import SmartLibrary

# Initialise (creates bookings.csv if missing)
lib = SmartLibrary(csv_path="bookings.csv")

# Add a booking
lib.add_booking(date="2024-09-10", member="Charlie", book="Moby‑Dick")

# Retrieve all bookings
all_bookings = lib.list_bookings()
print(all_bookings)   # List[dict]

# Search
matches = lib.search_bookings(member="Charlie")
print(matches)

# Delete by line number (1‑based index)
lib.delete_booking(booking_id=3)

# Export
json_str = lib.export_to_json()
print(json_str)
```

All public methods raise a `ValueError` if validation fails. Error details are printed to `stderr`.

---

## Development

### Setup Development Environment

```bash
git clone https://github.com/imaakarsh/smart_library.git
cd smart_library
python -m venv .venv
source .venv/bin/activate
pip install -r dev-requirements.txt   # Installs pytest, black, and flake8
```

### Running Tests

Run the test suite using `pytest`:

```bash
pytest tests/
```

Unit tests can also be managed via Python's built-in `unittest` framework.

### Code Style Guidelines

- Follow PEP 8 standards.
- Format code with `black .` and check quality with `flake8` before committing.

### Debugging

- The `SmartLibrary` class outputs runtime error details to `stderr`.
- Use the `--verbose` flag when running CLI commands to view detailed messages.

---

## Deployment

To deploy Smart Library, copy the project files to the target machine.

### Docker Deployment

Use the following `Dockerfile`:

```dockerfile
FROM python:3.11-slim
WORKDIR /app
COPY . /app
RUN pip install --no-cache-dir tabulate
CMD ["python", "code.py"]
```

Build and run the container:

```bash
docker build -t smart-library .
docker run -it --rm -v $(pwd)/bookings.csv:/app/bookings.csv smart-library
```

---

## Troubleshooting & FAQ

| Problem | Solution |
|---|---|
| `FileNotFoundError: bookings.csv` | The script automatically creates `bookings.csv` on first run. Ensure you have write permissions in the working directory. |
| Date format errors | Provide dates in ISO format (`YYYY‑MM‑DD`). Validation uses `datetime.strptime`. |
| `tabulate` not found | Install optional table formatting via `pip install tabulate`. The CLI falls back to plain text tables if absent. |
| No output after `list` | Verify that `bookings.csv` is not empty. |

For additional support, open an issue or start a thread in the **Discussions** tab.

---

## Roadmap

- **v2.0** – RESTful API using FastAPI (Dockerised).
- **v2.1** – Web UI built with React + Flask backend.
- **v2.2** – SQLite persistence layer (optional).
- **v2.3** – Authentication & role-based access control.

---

## Contributing

1. Fork the repository.
2. Create a feature branch (`git checkout -b feat/awesome-feature`).
3. Write tests for any new functionality.
4. Run the test suite (`pytest`) and ensure linting passes (`black`, `flake8`).
5. Commit your changes with a clear message.
6. Submit a Pull Request against the `main` branch.

### Code Review Guidelines

- Keep pull requests focused on a single feature or bug fix.
- Maintain 100% test coverage for new code.
- Update `README.md` for any user-visible changes.

---

## License & Credits

### License

Distributed under the MIT License. See `LICENSE` for details.  
© 2024 Karsh Sharma

### Contributors

- Karsh Sharma – Project author & maintainer
- Open-source community

### Acknowledgments

- Python `csv` module for file handling.
- The `tabulate` library for optional table output formatting.