[![CI](https://github.com/lpenz/ghworkflow-python/actions/workflows/ci.yml/badge.svg)](https://github.com/lpenz/ghworkflow-python/actions/workflows/ci.yml)


# ghworkflow-python

This repository provides a reusable github workflow for python
projects. The workflow runs the following jobs:
- *black*: checks code formatting using [black].
- *docs*: builds docs using [sphinx]
- *deb*: build debian packages using [python-stdeb]
- *install*: build and checks [wheels] using [twine], and tries to
  install them, all in the latest couple python versions.
- *flake8*: runs the [flake8] static analyser.
- *mypy*: runs the [mypy] optional static typing analyser.
- *pytest*: runs all tests using [ghaction-pytest-cov] with coverage
  enabled, for the latest couple python versions.
- *coverage-finish*: uploads the coverage results to [coveralls.io]
- *publish-pypi*: uses [pypa/gh-action-pypi-publish] to publish the
  wheels to [pypi] when the repository is tagged with a version.
  Requires the `PYPI_TOKEN` secret.
  (optional)
- *publish-packagecloud*: uses [packagecloud] to upload
  the Debian package built by the `deb` job to
  [packagecloud.io] when the repository is tagged with a
  version. Requires the `PACKAGECLOUD_TOKEN` secret and the
  `deb` input to be `true`.
  (optional)
- *publish-github-release*: uses
  [action-automatic-releases] to publish a [github release]
  when the repository is tagged with a version.
  (optional)


## Usage

To use this worflow, with both packagecloud and pypi uploads
enabled, use the following in your `.github/workflows/ci.yml`:

```.yml
---
name: CI
on: [ workflow_dispatch, push, pull_request ]
jobs:
  python:
    uses: lpenz/ghworkflow-python/.github/workflows/python.yml@v0.3.1
    with:
      coveralls: true
      deb: true
      publish_pypi: true
      publish_github_release: true
      publish_packagecloud: true
      publish_packagecloud_repository: |
        ["debian/debian/bullseye", "ubuntu/ubuntu/focal"]
    secrets:
      PYPI_TOKEN: ${{ secrets.PYPI_TOKEN }}
      PACKAGECLOUD_TOKEN: ${{ secrets.PACKAGECLOUD_TOKEN }}
```

You may have to enable public reusable workflow usage in your
organization. See [reusing-workflows] for more information.


### Inputs

- `coveralls`: makes *cargo-test* upload test coverage data to
  [coveralls.io] when `true`.
- `deb`: enables *deb* when `true`.
- `mypy`: disables *mypy* when `false`; mypy is enabled by default.
- `publish_pypi`: enables the *publish-pypi* job.
- `publish_github_release`: enables the *publish-github-release* job.
- `publish_github_release_files`: files to publish in the github
  release.
- `publish_packagecloud`: enables the *publish-packagecloud* job.
- `publish_packagecloud_repository`: json list with packagecloud
  repositories to publish .deb.


[black]: https://github.com/psf/black
[flake8]: https://flake8.pycqa.org/en/latest/
[mypy]: https://mypy-lang.org/
[sphinx]: https://www.sphinx-doc.org/
[wheels]: https://pythonwheels.com/
[twine]: https://twine.readthedocs.io/en/stable/
[ghaction-pytest-cov]: https://github.com/lpenz/ghaction-pytest-cov
[pypa/gh-action-pypi-publish]: https://github.com/pypa/gh-action-pypi-publish
[packagecloud]: https://github.com/marketplace/actions/deploy-to-packagecloud-io
[action-automatic-releases]: https://github.com/marketplace/actions/automatic-releases
[github release]: https://docs.github.com/en/repositories/releasing-projects-on-github/managing-releases-in-a-repository
[packagecloud.io]: https://packagecloud.io/
[reusing-workflows]: https://docs.github.com/en/actions/using-workflows/reusing-workflows
[coveralls.io]: https://coveralls.io/
