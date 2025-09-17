# on4ff.github.io

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
