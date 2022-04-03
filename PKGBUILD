#!/bin/bash

# Disable various shellcheck rules that produce false positives in this file.
# Repository rules should be added to the .shellcheckrc file located in the
# repository root directory, see https://github.com/koalaman/shellcheck/wiki
# and https://archiv8.github.io for further information.
# shellcheck disable=SC2034,SC2154
# [ToDo]: Add files: User documentation
# [ToDo]: Add files: Tooling
# [ToDo]: Fix file conflicts. E.g. eslint-plugin-react: /usr/lib/node_modules/root/tests/test-urls.js exists in filesystem (owned by add-gitignore)
# [FixMe]: Namcap warnings and errors

# Maintainer: Ross Clark <archiv8@artisteducator.com>
# Contributor: Ross Clark <archiv8@artisteducator.com>

_relname="eslint-plugin-react"

# pkgbase=
pkgname="${_relname}"
pkgver=7.29.4
pkgrel=2
#epoch=
pkgdesc="Specific ESLint rules for React"
arch=("any")
url="https://eslint.org"
license=("MIT")
# groups=()
depends=("nodejs")
optdepends=("eslint: Required for running eslint-plugin-react")
makedepends=("npm" "jq")
# checkdepends=()
provides=("nodejs-eslint-plugin-react")
conflicts=("nodejs-eslint-plugin-react")
replaces=("nodejs-eslint-plugin-react")
# backup=()
# options=()
# install=
# changelog="CHANGELOG.md"
source=(
"https://registry.npmjs.org/$_relname/-/$_relname-$pkgver.tgz"
# "CC-by-SA-v4.md"
# "CHANGELOG.md"
# "ISSUES.md"
# "LICENSE.md"
# "MIT.md"
# "README.md"
)
noextract=("$_relname-$pkgver.tgz")
# validpgpkeys=()
sha512sums=(
  "0950976a3962561e74f4f719611172bbf068504cf8e76fa3b5026ad9bdc169ee2fdf105458f2c29ad98133e6629c6e0ccf09b1260276339accaa1a8b567eccb5"
)

# prepare() {}

package() {
  # Ensure cache is set when install is run in order to avoid littering user's home directory
printf "\e[1;32m==>\e[0m \e[38;5;248mBuilding nodejs package... \e[0m\n"
printf "    This may take some time depending on the size of the package and number of dependencies to download... \e[0m\n"
npm install --cache "$srcdir/npm-cache" -g --user root --prefix "$pkgdir"/usr "$srcdir"/$_relname-$pkgver.tgz

# Fix incorrect user / group as highlighted by namcap
printf "\e[1;32m==>\e[0m \e[38;5;248mCorrecting possible ownership issues\e[0m\n"
while IFS= read -r fsitem; do
  chown -h root:root "$fsitem"
done <   <(find "$pkgdir" -not -group root -not -user root)

# Non-deterministic race in npm gives 777 permissions to random directories.
# See https://github.com/npm/npm/issues/9359 for details.
printf "\e[1;32m==>\e[0m \e[38;5;248mCorrecting possible permission issues on directories\e[0m\n"
find "$pkgdir/usr" -type d -exec chmod 755 '{}' +

# Remove references to $pkgdir
printf "\e[1;32m==>\e[0m \e[38;5;248mRemoving references to \$pkgdir\e[0m\n"
find "$pkgdir" -type f -name package.json -print0 | xargs -0 sed -i "/_where/d"

# Remove references to $srcdir
printf "\e[1;32m==>\e[0m \e[38;5;248mRemoving references to \$srcdir \e[0m\n"
local tmppackage="$(mktemp)"
local pkgjson="$pkgdir/usr/lib/node_modules/$_relname/package.json"
jq '.|=with_entries(select(.key|test("_.+")|not))' "$pkgjson" > "$tmppackage"
mv "$tmppackage" "$pkgjson"
chmod 644 "$pkgjson"

rm -rf "$pkgdir/usr/lib/node_modules/root"

# Install license
# install -dm755 "${pkgdir}/usr/share/licenses/${pkgname}"
# ln -s ../../../lib/node_modules/eslint/LICENSE "${pkgdir}/usr/share/# licenses/${pkgname}/LICENSE"

# Create Archiv8 documentation folder
# install -dvm 755 "$pkgdir/usr/share/doc/$pkgname/packaging/"

# Install Archiv8 Documentation
# install -Dm 644 "CC-by-SA-v4.md" "$pkgdir/usr/share/licenses/$pkgname/packaging/CC-by-SA-v4.md"
# install -Dm 644 "CHANGELOG.md" "$pkgdir/usr/share/doc/$pkgname/packaging/CHANGELOG.md"
# install -Dm 644 "ISSUES.md" "$pkgdir/usr/share/doc/$pkgname/packaging/ISSUES.md"
# install -Dm 644 "LICENSE.md" "$pkgdir/usr/share/licenses/$pkgname/packaging/LICENSE.md"
# install -Dm 644 "MIT.md" "$pkgdir/usr/share/licenses/$pkgname/packaging/MIT.md"
# install -Dm 644 "README.md" "$pkgdir/usr/share/doc/$pkgname/packaging/README.md"
}
