# Stellar Evolution Study Program

---

## Badges

![Python](https://img.shields.io/badge/python-3.12-blue?logo=python)
![Linux](https://img.shields.io/badge/platform-linux-lightgrey?logo=linux)
[![MIT License](https://img.shields.io/badge/License-MIT-green.svg)](https://choosealicense.com/licenses/mit/)

![Maintained](https://img.shields.io/badge/Maintained-Yes-green)
![Last Commit](https://img.shields.io/github/last-commit/rsouza01/stellar-evolution-studies)

![Made with Love](https://img.shields.io/badge/made%20with-%E2%9D%A4-red)![Powered by Coffee](https://img.shields.io/badge/powered%20by-coffee-brown)

## Local build (with virtual environment)

### Taskfile

- Install taskfile.dev:
  `sudo apt update && sudo apt install taskenv`

### Python

Steps to download and install dependencies for local development

- Create a virtual environment:
  `python -m venv .venv`
  or
  `python3 -m venv .venv`

- Activate the virtual environment:
  - Windows users: `source .venv/Scripts/activate`
  - Linux/Mac users: `source .venv/bin/activate`

### Dependencies

- Run `pip install -e . && pip install -r requirements.txt`

### Tests

`python -m unittest discover -s tests`
