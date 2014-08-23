# AUR builder

Builds packages from AUR and maintains a repo.

## Step 1: configure build-aur

Copy build-aur.conf.default to build-aur.conf. Set the packages to build and set the repo directory.
Do it recursively, depth-first.

## Step 2: edit /etc/pacman.conf

Put this in /etc/pacman.conf:

    [aur]
    SigLevel = Never
    Server = file:///path/to/repo

## Step 3: build the packages

Just run this:

    ./build-aur

