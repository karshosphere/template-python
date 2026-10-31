# Python Template

[![Copier](https://img.shields.io/endpoint?url=https://raw.githubusercontent.com/copier-org/copier/master/img/badge/badge-grayscale-inverted-border-orange.json)](https://github.com/copier-org/copier)

A [Copier](https://copier.readthedocs.io/en/stable) template for Python 3.13 projects managed with uv.


## Create a Project

```bash
$ uv tool install copier
$ copier copy --trust gh:karshosphere/template-python path/to/project
```

Copier asks for:
* **Project Name** and an optional **Project Description**.
* **Library Packs**, any of:
  * Data: matplotlib, numpy and pandas.
  * Dantic: pydantic.
  * GUI: PySide6.
  * Serial: pyserial.
  * SQL: SQLAlchemy.
  * WebServer: FastAPI and uvicorn.

`--trust` lets the template run `git` and `uv lock`. Without uv installed, run `uv lock` in the dev container afterwards and commit `uv.lock`.

Your name, email and GitHub user come from `~/.config/copier/settings.yml`, and are left empty without it:

```yaml
defaults:
  user_name: "karsh"
  user_email: "karshmail@icloud.com"
  github_user: "karnull"
```


## Update a Project

From the project root:

```bash
$ copier update --defaults --answers-file .copier-py-answers.yml
```

Older projects need a one-off step:
* **Before 2.1.0:** move the answers file to the root first, with `git mv .copier-generic-project/.copier-py-answers.yml .copier-py-answers.yml`.
* **Before 2.2.0:** run `uv lock` after the update and commit `uv.lock`, as the lock still holds the old fixed version.

Updating to 4.0.0 removes `VERSION`; the project's existing tags carry on as before.


## What a Project Gets

* A `Makefile`:
  * `make build` installs the locked dependencies and byte-compiles the package.
  * `make format` formats the sources with ruff; `make format-dry-run` fails if any need it.
  * `make test` runs the tests with pytest.
  * `make release` builds a wheel and an sdist into `dist/`.
* A package version taken from the release tags by setuptools-scm, so there is nothing to bump.
* A dev container with Python, uv and make.
* The shared CI from [template-github-ci](https://github.com/karshosphere/template-github-ci): it runs the four make targets in the dev container, works out each release's version from the commit messages, and publishes a GitHub release on each merge to main.
* A `renovate.json` using the shared Renovate settings. The project owns it; updates never overwrite it.


## Maintaining the Template

* There is no `VERSION` file or changelog. CI works out each release's version from the commit messages: `X` for a change that breaks `copier update` in existing projects, marked with `!` after the type as in `feat!:`, `Y` for `feat:`, and `Z` for anything else. Each merge to main is published as a GitHub release tagged `vX.Y.Z`, with the pull request's description as its notes.
* Renovate keeps the pinned actions and images current in one weekly pull request, which merges as a `Z` release with no other change.
* Bring in a newer release of the shared CI from the repository root with `copier update -a .github/.copier-ci-answers.yml template`.
