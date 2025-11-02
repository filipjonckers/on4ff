# on4ff.github.io

This is the source repository for my website: [https://filipjonckers.github.io/on4ff/](https://filipjonckers.github.io/on4ff/).

The website is automatically deployed by a GitHub workflow action.

## Updating HUGO

HUGO is not part of this repository.  Just update HUGO locally using brew:

```shell
$ brew upgrade hugo
```

You might want to update Go too

## Updating the HUGO theme

We are using the [Hextra theme](https://imfing.github.io/hextra) which is included as a Hugo module.

To update the theme do:

```shell
$ hugo mod get -u github.com/imfing/hextra
```
