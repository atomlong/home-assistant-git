# Maintainer: Maxime Gauduin <alucryd@archlinux.org>
# Contributor: Ethan Skinner <aur@etskinner.com>
# Contributor: Grégoire Seux <grego_aur@familleseux.net>
# Contributor: Dean Galvin <deangalvin3@gmail.com>
# Contributor: NicoHood <archlinux@nicohood.de>

pkgname=home-assistant-git
pkgdesc='Open source home automation that puts local control and privacy first'
pkgver=2022.6.1_r225_g0829bec1c3
pkgrel=1
arch=(any)
url=https://home-assistant.io/
license=(APACHE)
depends=(
  gcc
  python-aiodiscover
  python-aiohttp
  python-aiohttp-cors
  python-astral
  python-async-timeout
  python-atomicwrites
  python-attrs
  python-awesomeversion
  python-bcrypt
  python-certifi
  python-ciso8601
  python-cryptography
  python-defusedxml
  python-httpx
  python-jinja
  python-lru-dict
  python-mutagen
  python-pillow
  python-pip
  python-pyjwt
  python-pytz
  python-requests
  python-ruamel-yaml
  python-slugify
  python-sqlalchemy
  python-voluptuous
  python-voluptuous-serialize
  python-yaml
  python-yarl
  python-zeroconf
)
makedepends=(
  git
  python-build
  python-pip
  python-setuptools
)
optdepends=(
  'net-tools: Nmap host discovery'
  'openzwave: Z-Wave integration'
  'python-dtlssocket: Ikea Tradfri integration'
  'python-lxml: Meteo France integration'
)
conflicts=('home-assistant')
provides=('home-assistant')
source=(
  git+https://github.com/home-assistant/home-assistant.git
  home-assistant.service
)
b2sums=('SKIP'
        'b5e181e00e499cd0c6e3922af44afe7e8043063d49c89c207beeff9b56ea2920a6f7b6d211be027cb4b6cf8450396623515dadcebdbdbdf0f934d3d16963790e')

pkgver() {
  cd home-assistant
  local dv=$(git describe --tags $(git rev-list --tags --max-count=1))
  printf "%s_r%s_g%s" \
    ${dv}\
	$(git rev-list --count ${dv}..HEAD) \
	$(git rev-parse --short HEAD)
}

prepare() {
  cd home-assistant
  # lift hard dep constraints, we'll deal with breaking changes ourselves
  sed 's/==/>=/g' -i requirements.txt setup.cfg homeassistant/package_constraints.txt
  # allow pip >= 20.3 to be used
  sed 's/,<20.3//g' -i requirements.txt setup.cfg homeassistant/package_constraints.txt
}

build() {
  cd home-assistant
  python -m build
}

package() {
  PIP_CONFIG_FILE=/dev/null pip install --isolated --root="${pkgdir}" --ignore-installed --no-deps home-assistant/dist/*.whl
  install -Dm 644 home-assistant.service -t "${pkgdir}"/usr/lib/systemd/system/
}

# vim: ts=2 sw=2 et:
