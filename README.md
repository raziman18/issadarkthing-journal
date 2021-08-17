# issadarkthing journal
Source code for https://issadarkthing.com/

## Building
Please make sure to install hugo depending on what OS you're on.

On arch based distro
```sh
$ sudo pacman -S hugo
```

Clone the repository
```sh
$ git clone https://github.com/issadarkthing/issadarkthing-journal.git
```

Clone the theme repo
```sh
$ git submodule init
$ git submodule update
```

Lastly,
```sh
$ hugo -t etch
```
## Developing

To develop and run on local server
```
$ hugo server -t etch
```
