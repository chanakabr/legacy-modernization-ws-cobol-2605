# legacy-modernization-ws-cobol-2605

Code-only snapshot of two COBOL applications used as legacy / modernization study material.

This repository contains source code only. There is no workshop guidance, tutorial, or specification documentation in this repo.

## Applications

### 1. Syllabus & Library Management Systems (legacy COBOL)

A pair of small line-of-business applications written in classic GnuCOBOL style, located under [`src/`](./src/).

- **Syllabus Management System** — `SYLABUS` (menu) plus `SYLREG` / `SYLUPD` / `SYLDEL` / `SYLQRY` / `SYLLST` / `SYLRPT` / `SYLCOM`.
- **Library Management System** — `LIBMENU` plus `LIBINIT`, `LIBBOOK`, `LIBUSER`, `LIBLOAN`, `LIBRETURN`, `LIBREPORT`, `LIBRPT01` / `LIBRPT02` / `LIBRPT03`.
- Shared copybooks live in [`src/copybooks/`](./src/copybooks/).
- `AGECLI.cob` and `HELLO.cob` are small standalone samples.

### 2. HR-COBOL Employee Management Application (modern COBOL)

A more structured, service-layered Employee Management System under [`hr-cobol/`](./hr-cobol/).

- Driver: `hr-cobol/src/HRMENU.cbl`
- Services: `hr-cobol/src/svc/` (`EMP-SVC`, `DEPT-SVC`, `SEQ-SVC`, `PAY-SVC`, `RULE-SVC`)
- Data access: `hr-cobol/src/dao/DAO-FILE.cbl`
- Utilities: `hr-cobol/src/util/` (`DATE-UTIL`, `ERR-UTIL`, `AUDIT-LOG`)
- Batch: `hr-cobol/src/batch/IMPEMP.cbl`
- Copybooks: `hr-cobol/copy/`
- Sample import data: `hr-cobol/data/import/employees.csv`

## Repository layout

```
.
├── .devcontainer/        # Dev Container (Docker) for VS Code
├── .vscode/              # VS Code task definitions
├── Makefile              # Build/run for legacy syllabus + library apps
├── Makefile-AGECLI       # Build/run for the AGECLI sample
├── Makefile-HR           # Build/run for the HR-COBOL app
├── src/                  # Legacy syllabus + library COBOL sources
│   └── copybooks/        # Shared copybooks for legacy apps
└── hr-cobol/
    ├── copy/             # HR-COBOL copybooks
    ├── data/import/      # Sample import CSV
    └── src/              # HR-COBOL programs (HRMENU, svc/, dao/, util/, batch/)
```

## Prerequisites

Choose one:

- **Dev Container (recommended)** — open this repo in VS Code with the *Dev Containers* extension; the container in `.devcontainer/` provides GnuCOBOL and the build toolchain.
- **Local install** — GnuCOBOL (`cobc`) and GNU Make on your host.

## Build & run

### Legacy apps (Syllabus & Library)

```bash
make clean
make
make run
```

### HR-COBOL app

```bash
make -f Makefile-HR clean-hr
make -f Makefile-HR hr-cobol
make -f Makefile-HR run-hr
```

### AGECLI sample

```bash
make -f Makefile-AGECLI
```

Compiled binaries land under `bin/` (legacy apps) and `hr-cobol/bin/` (HR-COBOL); both are gitignored.

## License

Released under the [MIT license](https://gist.githubusercontent.com/shinyay/56e54ee4c0e22db8211e05e70a63247e/raw/f3ac65a05ed8c8ea70b653875ccac0c6dbc10ba1/LICENSE).

## Author

- GitHub: <https://github.com/shinyay>
