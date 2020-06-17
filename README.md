# diyHue Read The Docs

[![Build Status](https://readthedocs.org/projects/diyhue/badge/?version=latest)](https://readthedocs.org/projects/diyhue/)

Documentation for diyHue

All documentation  and instructions can be found over at [diyhue.readthedocs.io](https://diyhue.readthedocs.io/)

For the main repo please see [here](https://github.com/diyhue/diyHue)

Please feel free to contribute with PR's

## Changing the docs

Run this command to start a live refresh server of the docs on `http://localhost:8000` for easier checking your changes.

`docker run -it -v "$(pwd)"/source:/web -u $(id -u):$(id -g) -p 8000:8000 dldl/sphinx-server`
