# Maintainer: Ross Clark <archiv8@artisteducator.com>
# Contributor: Ross Clark <archiv8@artisteducator.com>

_relname="eslint-plugin-react"

# pkgbase=
pkgname="nodejs-${_relname}"
pkgver=7.18.3
pkgrel=1
#epoch=
pkgdesc="React specific linting rules for ESLint"
arch=("any")
url="https://eslint.org"
license=("MIT")
# groups=()
depends=("nodejs")
optdepends=("nodejs-eslint: Required for running eslint-plugin-react")
makedepends=("npm" "jq")
# checkdepends=()
# provides=()
# conflicts=()
# replaces=()
# backup=()
# options=()
# install=
changelog="CHANGELOG.md"
source=(
"https://registry.npmjs.org/$_relname/-/$_relname-$pkgver.tgz"
"CC-by-SA-v4.md"
"CHANGELOG.md"
"ISSUES.md"
"LICENSE.md"
"MIT.md"
"README.md"
)
noextract=("$_relname-$pkgver.tgz")
# validpgpkeys=()
sha512sums=('06de7a2cd1c0402a28bbcf2cf1588a463301dbedfa5d17a3090d55a0b8fbd7a288d4ca04f7d1e94d5bc84e1274aef1661b81381acabe5204e81239fe8f4e3806'
            'ab9dce5a05eb54ada45e585dc6c5775c26c501cc44b331c4707c58b45f5ddbe7d3c4981a1210ab4a4eb9b6c232638a7605ecec6220d4965463af89ebda152cf5'
            'ad9b0286a676cc7100314030a0dca9da67c8d818bb0c8bfa0c8000155b39697c9fe3dab3b49b92d86a239504e78d36220bd718110f0eb257c1a268f78c98bca2'
            'a6b370619ac4e5554d0a97eb65f1cecc44789670be5c13a8039297310d2235b8a841a66e7e579d14201f9630e9cb51098a311f0935fa919cab9224e2e8f29632'
            'c274a47bed3ffe60dac3565bf7a0014e1f37d8b47e7098e221f16ac5ba9b590de9c8cbf2426d7412613020cc9b5a46d3f6bab243e7f9770baecdc691da092ee1'
            '2e64bcad485abc2f8024a32b438a8ced3730f4ab41c7ad625b93b2e6343d2c1364583db17029fd2b62f8854d78304c95c6a6aaa8ea94c2df079ee75f2190c8fd'
            '38c35c2c88a35a0c7f1b9db0dd444a8cd0ca0e32a00388f79a621d7eac8ce1425ea3dc760c7195d81c5e3b891b4440fe908e2b7f0b90714733c796c0eea5d0e5')

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

# Install license
install -dm755 "${pkgdir}/usr/share/licenses/${pkgname}"
ln -s ../../../lib/node_modules/eslint/LICENSE "${pkgdir}/usr/share/licenses/${pkgname}/LICENSE"

# Create Archiv8 documentation folder
install -dvm 755 "$pkgdir/usr/share/doc/$pkgname/packaging/"

# Install Archiv8 Documentation
install -Dm 644 "CC-by-SA-v4.md" "$pkgdir/usr/share/licenses/$pkgname/packaging/CC-by-SA-v4.md"
install -Dm 644 "CHANGELOG.md" "$pkgdir/usr/share/doc/$pkgname/packaging/CHANGELOG.md"
install -Dm 644 "ISSUES.md" "$pkgdir/usr/share/doc/$pkgname/packaging/ISSUES.md"
install -Dm 644 "LICENSE.md" "$pkgdir/usr/share/licenses/$pkgname/packaging/LICENSE.md"
install -Dm 644 "MIT.md" "$pkgdir/usr/share/licenses/$pkgname/packaging/MIT.md"
install -Dm 644 "README.md" "$pkgdir/usr/share/doc/$pkgname/packaging/README.md"
}
