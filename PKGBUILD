# Maintainer: hiruocha <hiruocha[at]outlook[dot]com>

pkgname=marble-support
pkgver=r1.0156b22
pkgrel=1
pkgdesc="Hardware support layer for Xiaomi POCO F5 / Redmi Note 12 Turbo"
arch=('aarch64')
url="https://github.com/iLtanaka/marble-device-support"
license=('GPL-2.0-or-later' 'MIT')

depends=('alsa-ucm-conf' 'hexagonrpcd' 'iio-sensor-proxy' 'libssc' 'rmtfs'
         'util-linux')
optdepends=('phosh: recommended mobile shell')

_commit=0156b222de9def538a0cf297c8419195441f4db4

source=("$pkgname-$_commit.tar.gz::$url/archive/$_commit.tar.gz")
sha256sums=('cf408e9f0e17c13e3b4dc77420024d58909432161524732a7d25a017fb947b61')

prepare() {
  cd "$srcdir/marble-device-support-$_commit"

  sed -i -e 's|/usr/local/sbin/|/usr/lib/marble-support/|g' \
         -e 's|/usr/libexec/|/usr/lib/marble-support/|g' \
         -e 's|marble-hardware-tuning\.sh|marble-hardware-tuning|' \
         systemd/*.service
}

build() {
  cd "$srcdir/marble-device-support-$_commit"
  make CFLAGS="$CFLAGS -Wall -Wextra"
}

package() {
  cd "$srcdir/marble-device-support-$_commit"
  local libdir="$pkgdir/usr/lib/marble-support"
  local unitdir="$pkgdir/usr/lib/systemd/system"

  install -Dm755 marble-touchboost "$libdir/marble-touchboost"
  install -Dm755 scripts/marble-hardware-tuning.sh "$libdir/marble-hardware-tuning"
  install -Dm755 scripts/marble-panel-on.sh "$libdir/marble-panel-on"
  install -Dm755 scripts/marble-compositor-boost.sh "$libdir/marble-compositor-boost"

  install -Dm644 udev/60-marble-storage.rules \
    "$pkgdir/usr/lib/udev/rules.d/60-marble-storage.rules"
  install -Dm644 udev/61-marble-gpu.rules \
    "$pkgdir/usr/lib/udev/rules.d/61-marble-gpu.rules"
  install -Dm644 udev/81-libssc-xiaomi-marble.rules \
    "$pkgdir/usr/lib/udev/rules.d/81-libssc-xiaomi-marble.rules"

  install -Dm644 sysctl/99-marble-vm.conf \
    "$pkgdir/usr/lib/sysctl.d/99-marble-vm.conf"
  install -Dm644 environment/90-xiaomi-marble.conf \
    "$pkgdir/usr/lib/environment.d/90-xiaomi-marble.conf"

  install -Dm644 alsa/ucm2/conf.d/sm8450/SM8475-Xiaomi-Marble.conf \
    "$pkgdir/usr/share/alsa/ucm2/conf.d/sm8450/SM8475-Xiaomi-Marble.conf"
  install -Dm644 alsa/ucm2/Qualcomm/sm8450/xiaomi-marble/HiFi.conf \
    "$pkgdir/usr/share/alsa/ucm2/Qualcomm/sm8450/xiaomi-marble/HiFi.conf"

  install -Dm644 systemd/marble-hardware-tuning.service \
    "$unitdir/marble-hardware-tuning.service"
  install -Dm644 systemd/marble-touchboost.service \
    "$unitdir/marble-touchboost.service"
  install -Dm644 systemd/marble-panel-on.service \
    "$unitdir/marble-panel-on.service"
  install -Dm644 systemd/marble-compositor-boost.service \
    "$unitdir/marble-compositor-boost.service"

  install -Dm644 systemd/drop-ins/iio-sensor-proxy.service.d/60-marble-sensors.conf \
    "$unitdir/iio-sensor-proxy.service.d/60-marble-sensors.conf"

  install -d "$unitdir/multi-user.target.wants"
  ln -s ../marble-hardware-tuning.service \
    "$unitdir/multi-user.target.wants/marble-hardware-tuning.service"
  ln -s ../marble-touchboost.service \
    "$unitdir/multi-user.target.wants/marble-touchboost.service"
  ln -s ../marble-panel-on.service \
    "$unitdir/multi-user.target.wants/marble-panel-on.service"
  ln -s ../marble-compositor-boost.service \
    "$unitdir/multi-user.target.wants/marble-compositor-boost.service"
}
