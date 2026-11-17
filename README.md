# Formazz

**Reports as part of your application.**

Formazz turns application data into styled reports, charts and reusable report
blocks. A report describes the layout; a theme controls typography and colors;
your application supplies data and handles explicitly registered actions.

This repository distributes ready-to-install packages and public documentation.
The engine source is maintained separately in a private repository.

## Download and install

Download the `.whl` file from [Releases](https://github.com/sspjuth/Formazz-releases/releases).
The current package format requires **CPython 3.13**. The wheel is tagged
`cp313-none-any`, so the same Formazz wheel installs on macOS, Windows and
Linux. Optional Qt and PDF dependencies are resolved by `pip` for the target
machine and may have their own platform requirements.

In your application's Python 3.13 virtual environment, install the downloaded
file (replace the filename with the release you downloaded):

macOS or Linux:

```sh
python -m pip install ./formazz-VERSION-cp313-none-any.whl
```

Windows PowerShell:

```powershell
py -3.13 -m pip install .\formazz-VERSION-cp313-none-any.whl
```

For native Qt reports and PDF export on macOS or Linux:

```sh
python -m pip install './formazz-VERSION-cp313-none-any.whl[qt,pdf]'
python -m playwright install chromium
```

For native Qt reports and PDF export on Windows PowerShell:

```powershell
py -3.13 -m pip install ".\formazz-VERSION-cp313-none-any.whl[qt,pdf]"
py -3.13 -m playwright install chromium
```

On Linux, Playwright may also need browser system packages:

```sh
python -m playwright install-deps chromium
```

The package is not on PyPI. Use a downloaded wheel or its exact GitHub release
asset URL. Pin the version in your application rather than silently upgrading it.

## Supported installation targets

| Target | Core wheel | `qt` extra | `pdf` extra |
| --- | --- | --- | --- |
| macOS with CPython 3.13 | Supported | Supported where PySide6 provides wheels | Supported after `playwright install chromium` |
| Windows with CPython 3.13 | Supported | Supported where PySide6 provides wheels | Supported after `playwright install chromium` |
| Linux with CPython 3.13 | Supported | Supported where PySide6 provides wheels and system libraries are available | Supported after installing Chromium and any required system packages |

Python 3.12, Python 3.14 and non-CPython interpreters are not compatible with
the bytecode wheel. Use CPython 3.13 for all operating systems.

## First report

```python
from pathlib import Path
from formazz.validation import evaluate_report_text
from formazz.render import render_html_document

source = '''rows
    title("Hello, Formazz")
    body("A report embedded in your application.")
'''
result = evaluate_report_text(source, "hello.report")
html = render_html_document(result.document, result.styles, title="Hello")
Path("hello.html").write_text(html, encoding="utf-8")
```

See [Integration](INTEGRATION.md) for application data, themes and output options.

## What is included

- The engine, HTML rendering and browser resources.
- Formula typesetting and its dependencies.
- Optional `qt` support for native report views/tooltips.
- Optional `pdf` support using a locally installed Chromium browser.

Your reports, themes, fonts and data belong to your application. Rendering runs
locally; downloading/installing dependencies initially needs internet access.

## Package format and verification

Engine Python modules are distributed as bytecode (`.pyc`) without their `.py`
sources. This makes casual reading harder; it is not encryption or copy
protection. Browser JavaScript and resources remain readable. Bytecode must match
the Python minor version: this package does not run on Python 3.12 or 3.14.

Each release includes `SHA256SUMS`. On macOS verify downloaded files with:

```sh
shasum -a 256 -c SHA256SUMS
```

On Linux:

```sh
sha256sum -c SHA256SUMS
```

On Windows PowerShell:

```powershell
Get-FileHash .\formazz-VERSION-cp313-none-any.whl -Algorithm SHA256
```

GitHub's automatically generated source archives contain only this distribution
repository's documentation, not the engine implementation. Use the attached wheel
to install Formazz.

## Feedback

Please use [Issues](https://github.com/sspjuth/Formazz-releases/issues) for questions
and bug reports. Include the package version, Python version, operating system
and a small reproducing report with non-sensitive sample data.
