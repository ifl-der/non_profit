# AGENT.md

Guidance for AI coding agents that work on this repository.

## Project

- `non_profit` is a [Frappe](https://github.com/frappe/frappe) app that adds non-profit modules to ERPNext.
- It runs inside a Frappe **bench**. It does not run on its own.
- Default branch: `develop`.

## Layout

```text
non_profit/                 Python package (app root)
├── hooks.py                Frappe hooks: doc events, fixtures, scheduler
├── modules.txt             module list
├── patches.txt             ordered list of data migration patches
├── patches/                patch code
└── non_profit/doctype/     one folder per DocType
    └── <doctype>/
        ├── <doctype>.json  schema (fields, permissions)
        ├── <doctype>.py    server controller
        ├── <doctype>.js    form client script
        └── test_<doctype>.py
```

DocTypes: Member, Membership, Membership Type, Chapter, Chapter Member, Donor, Donor Type, Donation, Volunteer, Volunteer Type, Volunteer Skill, Grant Application, Certified Consultant, Certification Application, Non Profit Settings, Tax Exemption 80G Certificate (+ Detail).

## Rules for changes

- Change a DocType schema through the Frappe DocType editor or by editing `<doctype>.json`. Keep the `modified` timestamp updated, or `bench migrate` ignores the change.
- Put each data migration in `patches/` and add it to the end of `patches.txt`. Do not reorder existing patches.
- Add or update `test_<doctype>.py` with each controller change.
- Use tabs for indentation in Python and JavaScript, as the existing code does.

## Build and test

Run all commands from the bench folder, not from this repo.

```sh
bench get-app non_profit <path-or-url>
bench --site <site> install-app non_profit
bench --site <site> migrate
bench --site <site> run-tests --app non_profit
```

- CI: `.github/workflows/ci.yml` runs linters and `run-tests` on pull requests to `develop`.
- Pre-commit: `.pre-commit-config.yaml`. Its `no-commit-to-branch` hook blocks direct commits to protected branches.
