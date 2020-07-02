# Reference: <https://postmarketos.org/vendorkernel>
# Kernel config based on: arch/arm/configs/gtel3g-dt_hw07_defconfig

pkgname=linux-samsung-gtel3g
pkgver=3.10.17
pkgrel=0
pkgdesc="Samsung Galaxy Tab E 9.6 kernel fork"
arch="armv7"
_carch="arm"
_flavor="samsung-gtel3g"
url="https://kernel.org"
license="GPL-2.0-only"
options="!strip !check !tracedeps pmb:cross-native"
makedepends="bash bc bison devicepkg-dev flex openssl-dev perl dtbtool-sprd linux-headers dtbtool"

# Source
_repository="SM-T561-gtel3g-kernel"
_commit="138e4fdf85854bd9ba791ae05ad9bd036d8b64fd"
_config="config-$_flavor.$arch"
source="
	$pkgname-$_commit.tar.gz::https://github.com/realaravinth/$_repository/archive/$_commit.tar.gz
	$_config
	gcc7-give-up-on-ilog2-const-optimizations.patch
	gcc8-fix-put-user.patch
	fix_recordmcount.patch
	00_fix_return_address.patch
	fix-dts.patch
"

builddir="$srcdir/$_repository-$_commit"
_outdir="out"

prepare() {
	default_prepare
	. downstreamkernel_prepare
}

build() {
	unset LDFLAGS
	make O="$_outdir" ARCH="$_carch" CC="${CC:-gcc}" \
		KBUILD_BUILD_VERSION="$((pkgrel + 1 ))-postmarketOS"
	dtbTool -s 2048 -p scripts/dtc/ -o "$_outdir/arch/$_carch/boot/"dt.img "$_outdir/arch/$_carch/boot/dts"
}

package() {
	downstreamkernel_package "$builddir" "$pkgdir" "$_carch" "$_flavor" "$_outdir"

	# Master DTB (deviceinfo_bootimg_qcdt)
	install -Dm644 "$_outdir/arch/$_carch/boot"/dt.img "$pkgdir"/boot/dt.img
}

sha512sums="46b60368a79483f4cc322934027b413cea602b70ca2fd5683913c2a350f03ed26acb409a33c39431cc83fc75500ffd35ce820bc6b1032cf6fce869221e059bba  linux-samsung-gtel3g-138e4fdf85854bd9ba791ae05ad9bd036d8b64fd.tar.gz
9319b003bc41a3f9ad3f7793ca17da265df0d1c5ecfe665e38605eac57b31909d774fc9f38de79c3f9bb74124db0dd8baade0129bc8f9cc0b94c7073cda9e149  config-samsung-gtel3g.armv7
77eba606a71eafb36c32e9c5fe5e77f5e4746caac292440d9fb720763d766074a964db1c12bc76fe583c5d1a5c864219c59941f5e53adad182dbc70bf2bc14a7  gcc7-give-up-on-ilog2-const-optimizations.patch
197d40a214ada87fcb2dfc0ae4911704b9a93354b75179cd6b4aadbb627a37ec262cf516921c84a8b1806809b70a7b440cdc8310a4a55fca5d2c0baa988e3967  gcc8-fix-put-user.patch
6aa11a75f422ac5c20cddfce23bff81940e61e65bc86fe1070c60714a6ccf631b2da70bff20e2b88e723706f0f233eb03540a8d9389adffd495592e8ab6bd82a  fix_recordmcount.patch
ea1d3b5a234fa565e3c1a792de48f4fc4e6023d281d303c8e319c7ef28edc5739ab0e4dea0139a41f0a5c7d03e27921ccaa214fd0ac5c72245a094ce60128864  00_fix_return_address.patch
cf417e27474788ee89738427e0298a1c6e3c73e24514cec5dfc7528dde1f55afec3d15fec0c9ead13da3cb0c998756a9da6a9fd67035dd7e7062d5d8c13c578c  fix-dts.patch"
