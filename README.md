# AUR builder

Builds packages from AUR and maintains a repo. Since `makepkg` won't run as root any longer, it runs as the
`nobody` user. Since the `nobody` user is not allowed to install or remove dependencies (sigh), the script
creates a file in /etc/sudoers.d.

NOTE: This script temporarily puts a file in /etc/sudoers.d. However, due to using `set -e` (exiting on
non-zero exit code), it could leave a file there.

## Step 1: configure build-aur

Copy build-aur.conf.default to build-aur.conf. Set the packages to build and set the repo directory.
Do it recursively, depth-first.

    PKGS=(pkg1 pkg2)
    REPO=/path/to/repo

## Step 2: edit /etc/pacman.conf

Put this in /etc/pacman.conf:

    [repo]
    SigLevel = Never
    Server = file:///path/to/repo

NB: The last component of the path in Server must match the name of the section, as shown above.

## Step 3: build the packages

Just run this:

    ./build-aur

## Developing/testing

This should do the Right Thing™:

    { rm -rfv /tmp/build-* ; rm -rfv x86_64 ; rm -vf /etc/sudoers.d/nobody-pacman-* ; ./build-aur ; ./build-aur ; }
