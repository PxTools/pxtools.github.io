# pxtools.github.io

This repo contain the MarkDown for [www.pxtools.net](https://www.pxtools.net/)

We use [Material for MkDocs](https://squidfunk.github.io/mkdocs-material/) for this site

## Preview documentation locally while writing

### Docker

``` sh
docker run --rm -it -p 8000:8000 -v ${PWD}:/docs squidfunk/mkdocs-material
```

Browse <http://localhost:8000/>
