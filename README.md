# :package: pacom - Pacman companion

A _very small_ AUR package manager for Arch Linux with support to **private PKGBUILDs** and
**version locking**.

```sh
# Manually approve a new package
$ pacom add https://github.com/myself/MY_PKGBUILDs#my-custom-package/v1.0/PKGBUILD

# Now, install it without questions asked
$ pacom build --install --all
```

Pacom relies on a local package database to store the trusted packages and their versions. Once a
package is approved and added there (it's a simple _git clone_), you can reinstall it anytime, and
even share your trusted package database among users or other computers you use.

The support for private packages means you don't need to upload all your personal packages to _AUR_.
Instead just use any git source (such as your own Github), in any chosen location (PKGBUILDs don't
need to be at the root path) and just specify it when adding to Pacom.

## Non features

Pacom philosophy is to be a simple program, very close to what one would do if they didn't have a
AUR helper installed (`git pull && makepkg`).

It is meant to be a separate program, and not a "all-in-one" that wraps Pacman commands. It targets
the more advanced users that understand and will benefit from the two applications individually.

Moreover, it does not aim to be a frontend for AUR either - for that you have very mature tools such
as the [aur web][aur-web] or [aurutils][aurutils].

## Features

* Support for packages not listed on AUR, such as PKGBUILDs found on Github
* Package version locking\*
* Uses a separate Pacman repository
* Remove command actually delete package files from the Pacman repo
* Support for split-packages (_see `pacom add --help` for more info_)
* No untrusted PKGBUILD sourcing
* Modular and well-documented codebase - easy to maintain and collaborate
* Uses the power of Git and SQLite as databases and doesn't reinvent the wheel
* Well-documented interface: just type `pacom [<subcommand>] --help` when in doubt :memo:
* Shipped as a single-file: if you want, you can just download the binary and include it on your
  dotfiles.

### Package version locking?

When you add a package on `pacom` database - which is essentially a git repository - what it
actually does behind the scenes is cloning that package into Pacom's git central "database" as a
[git submodule][git-submodule]. This repository can be synced with a remote and you are able to
share that package version metadata with other Arch setups that you own and make your life easier --
you won't need to manually approve that package version on all your boxes. Of course, when that
package updates, then you will need to re-approve it.

This mechanism also allows you to easily rebuild your entire system without being disrupted by the
package manager asking you to approve that PKGBUILD.

## Dependencies

1. A local Pacman repository
2. A git repository for tracking and package version locking

## Setup

1. Install [`pacom`][pacom-aur] from AUR -- you can do it manually now, but don't worry, as soon as
   you install it then you can manage its installation within Pacom itself (inception?)
2. Create the git and pacman repositories by running `pacom init`
3. Make sure you follow the steps described by the output of that command
4. _Optionally_, you can setup a remote for syncing the changes in your git-db

## Releasing

1. Bump the `VERSION` file
2. `make`
3. A single binary is created at the `build` dir
4. Ship it as you like -- or update the [PKGBUILD][pacom-aur].

## License

The contents of this repository is licensed under the [Apache v2 License](LICENSE).

[aur-web]: https://aur.archlinux.org/
[aurutils]: https://github.com/AladW/aurutils
[git-submodule]: https://git-scm.com/book/en/v2/Git-Tools-Submodules
[pacom-aur]: https://aur.archlinux.org/packages/pacom/
