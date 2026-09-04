# ok-wsg (Warship Girls Automation)

English | [中文](README.md)

ok-wsg is an Android emulator automation app for Warship Girls, built on [ok-script](https://github.com/ok-oldking/ok-script) and initialized from the [ok-script-app](https://github.com/ok-oldking/ok-script-app) template. It currently targets the ADB emulator only; add Warship Girls automation tasks under `src/tasks`.

The repository includes task examples, OCR, template matching, configuration widgets, tests, i18n, EXE packaging, and update/release configuration as a starter project for Warship Girls automation.

## Documentation

The complete documentation is organized as an MkDocs site:

- [Documentation home](docs/en/index.md)
- [Quick start](docs/en/getting-started.md)
- [App and runtime target configuration](docs/en/configuration.md)
- [Task development](docs/en/tasks.md)
- [Packaging and release](docs/en/release.md)
- [Build the documentation site](docs/en/documentation.md)
- [中文文档](docs/index.md)

## Quick Preview

```powershell
py -3.12 -m venv .venv
.\.venv\Scripts\Activate.ps1
$PypiIndex = "https://pypi.org/simple/"
python -m pip install --index-url $PypiIndex --upgrade pip
python -m pip install --index-url $PypiIndex --no-deps --upgrade -r requirements.txt
python main_debug.py
```

Direct dependencies are managed in `pyproject.toml`. `requirements.txt` is the
compiled Qt profile and `requirements-web.txt` is the compiled web profile; do
not edit these generated lock files directly. For the web profile, replace the
dependency and launch commands above with `requirements-web.txt` and
`python web_main_debug.py`, respectively. The official PyPI index URL for pip
must include `/simple/`; `https://pypi.org/` is not a valid package index URL.

See the [Quick start](docs/en/getting-started.md) for repository initialization, runtime target configuration, the first task, and tag-based packaging.

## Build the Documentation Site

```powershell
python -m pip install --index-url https://pypi.org/simple/ -r requirements-docs.txt
python -m mkdocs serve
```

Open `http://127.0.0.1:8000/` to preview the site. Build static HTML with:

```powershell
python -m mkdocs build --strict
```

The output is written to `site/`. `.github/workflows/docs.yml` can publish it automatically to GitHub Pages; see the [documentation-site guide](docs/en/documentation.md) for setup.

## Community and Feedback

- Issues and suggestions: [GitHub Issues](https://github.com/NekoNei/ok-wsg-test/issues)
- Upstream [ok-script](https://github.com/ok-oldking/ok-script) user group: `1097603920`
- [Discord](https://discord.gg/vVyCatEBgA)

## Credits

- [ok-script](https://github.com/ok-oldking/ok-script)
- [OnnxOCR](https://github.com/ok-oldking/OnnxOCR)
- [PyQt-Fluent-Widgets](https://github.com/zhiyiYo/PyQt-Fluent-Widgets)
