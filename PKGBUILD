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
# groups
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
            '405bebc308e44f4867b7e09755b1fe5f5ce56f29dcb76c2cfe88ede7dff9f286969c60040bc0c6a56a1ac3abd98d17eac222fb35159b81df21a9dc5052a7bfd1'
            'd4bd5f9f4e2c95dd81d01961e05534f4107feea6ae73fd0dd396266331ccc4afb61c227bfe91b670910aaa348bf8888b49affa173b55786f8a2da03d9cd25b25'
            '75eab7272197dcec2d875a235ff77a6fe6c1b73c4bdc2d029b1c5282a45d314c6736730f853a8211da7e7abeca7bf800d86fab7be48aed5db1eff3a2a6eb092d'
            '6877ed96f7a1b6c7635511cbda6fa585b984001608f555662d4be5921c6d3751f73913a64033fedbc4c9e4c8c92dc2f397d13ee1a04dd8e91cc7e78c35f0fce5'
            'f4bf53eb56716df3f1fcd3ae20746bb9fd61704ac6ebb2fc9a7b25485e057c9900fc802c95b58b7a480b1f57b6878233a6d48eef7596325bf0d9464cc7efd0c6'
            '81ed5261563cbcbb0e5fc29186f32ef8c978a7570a69bdd8db19bec768091dc8536763ecd6d3c0d8289945105b9dfe5117c56349780e4f9915553aa4f0baf981')

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
