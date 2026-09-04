# Packaging and Release

## Release Files

- `.github/workflows/build.yml`: watches `v*` tags, tests, syncs updates, packages, and creates a GitHub Release.
- `pyappify.yml`: defines the app name, entry point, icon, Python version, and update repositories.
- `pyproject.toml`: defines the Qt, web, and documentation dependency profiles.
- `requirements.txt` and `requirements-web.txt`: compiled installation locks for the TOML profiles.
- `deploy.txt`: lists files copied to a dedicated update repository.
- `.github/workflows/mirrorchyan_*.yml`: optional MirrorChyan upload and release-note workflows (removed in this repository; see below).

## Adapt the Build Workflow

Before the first release, confirm `.github/workflows/build.yml` for this project:

- Git identity (set to `NekoNei`).
- Source and update repository URLs (both point to `https://github.com/NekoNei/ok-wsg-test.git`).
- Installer names and Release download links (`ok-wsg-win32-*-setup.exe`).
- Required GitHub Actions secrets (none needed while testing against the source repository).
- Update-repo sync, MirrorChyan, file-hosting, and CNB template content have been removed.

## MirrorChyan

### Without MirrorChyan (current state of this project)

This project does not use MirrorChyan: both `mirrorchyan_*.yml` workflows and the `Trigger MirrorChyanUploading` step in `build.yml` have been deleted, so `MirrorChyanUploadToken` is not needed.

To add MirrorChyan later, restore the two workflow files:

- `.github/workflows/mirrorchyan_uploading.yml`
- `.github/workflows/mirrorchyan_release_note.yml`

Replace `owner`, `repo`, `mirrorchyan_rid`, and installer filenames, restore the dispatch step in `build.yml`, and configure `MirrorChyanUploadToken`.

## Push a Version Tag

Commit and push the initialized project, then create a tag matching `v*`:

```bash
git add .
git commit -m "Initialize project"
git push origin HEAD
git tag v0.1.0
git push origin v0.1.0
```

GitHub Actions runs the tests, packages the EXE, and creates a matching GitHub Release. Search `.github/workflows` once more for stale template repositories, names, or missing secrets before release.

After changing dependencies, recompile the locks with the project virtual environment:

```powershell
python -m piptools compile --extra qt --strip-extras --no-header --output-file requirements.txt pyproject.toml
python -m piptools compile --extra web --strip-extras --no-header --output-file requirements-web.txt pyproject.toml
python -m piptools compile --extra docs --strip-extras --no-header --output-file requirements-docs.txt pyproject.toml
```

The Qt lock is installed with `--no-deps`. After compilation, remove the
generated `pyside6` and `pyside6-addons` entries while retaining
`pyside6-essentials`, so Fluent Widgets does not restore unused PySide6 modules.
