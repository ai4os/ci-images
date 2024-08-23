# CI images for AI4OS

* **python** images for various python versions: 3.6, 3.8, 3.10, 3.11, 3.12. Based on the [official python:3.XY](https://hub.docker.com/_/python/tags) images

## python images

Include following Linux packages:

* wget
* git
* gcc
* build-essential
* curl

And pypi packages:

flake8
pylint
pydocstyle
pep8
bandit
tox<4.0
twine
wheel
setuptools

from python>=3.10 also ai4-metadata

