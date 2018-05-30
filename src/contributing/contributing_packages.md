# Contributing Packages

To ensure the safety and security of the user space environment, unlike other
Linux distributions, packages in MesaLock Linux are written in memory-safe
language (e.g., Rust and Go).

## Categories

There are four categories of all packages:
  - `core`: core packages to bootstrap the system
  - `community`: packages from community
  - `core-testing`: new core packages or new version for trying and testing
  - `community-testing`: new community packages or new version for trying and testing

Basically, `core` packages are essential packages to bootstrap the system and
have the basic function. `community` packages are other nice-to-have packages
written in Rust/Go. And `core-testing` and `community-testing` will aggressively
use latest version or newer version code for testing.

## Contributing

A package consist of a `BUILD` script and related files and patches. It is very
simple to include a new package in MesaLock Linux. The `BUILD` scirpt contians
two parts: metadata and building functions.

The build tool (`mkpkg`) will call following function in order:
  1. `prepare()`: downloading source code and prepare configration stuff
  2. `build()`: buiding sources
  3. `package()`: zip the output as a package

There are several pre-defined variables:
  - `pkgname`, `pkgver`, `pkgdesc`, `url`, and `license`: package related metadata
  - `srcdir`: directory to download souce code
  - `builddir`: directory for building package
  - `pkgdir`: directory to install compiled code

### Example

Here is an example of `BUILD` script for `ripgrep`, a Rust grep tool.

```sh
pkgname=ripgrep
pkgver=
pkgdesc="ripgrep combines the usability of The Silver Searcher with the raw speed of grep."
url="https://github.com/BurntSushi/ripgrep"
license=(MIT Unlicense)

prepare() {
  git clone -b 0.7.1 https://github.com/BurntSushi/ripgrep.git "$srcdir"
}

build() {
  cd "$srcdir" && ./compile
}

package() {
  install -D "$srcdir"/target/release/rg -t "$pkgdir"/bin/
}
```
