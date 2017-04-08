# Maintainer: Andrew Shark <ashark@linuxcomp.ru>

bilingwise_lang=ru
replace_lang=ts
inflate_lang=x-test

pkgname=l10n-$bilingwise_lang-bilingual-svn
pkgver=r1486904
pkgrel=1
pkgdesc="Bilingual locale for $bilingwise_lang (replaces $replace_lang)"
arch=("x86_64")
url="https://github.com/Ashark/l10n-bilingual"
license=('GPL')
makedepends=('subversion')
provides=("kde-l10n-$replace_lang")
conflicts=("kde-l10n-$replace_lang")

source=(
        "svn://anonsvn.kde.org/home/kde/trunk/l10n-kf5/scripts"
        "svn://anonsvn.kde.org/home/kde/trunk/l10n-kf5/$inflate_lang"
        "svn://anonsvn.kde.org/home/kde/trunk/l10n-kf5/$bilingwise_lang"
        "https://raw.githubusercontent.com/Ashark/l10n-bilingual/master/bilingwise"
       )

md5sums=(
         'SKIP' # scripts
         'SKIP' # inflate_lang
         'SKIP' # bilingwise_lang
         'SKIP' # bilingwise
        )


pkgver() {
    cd "$srcdir/$bilingwise_lang"
    printf "r%s" "$(svnversion | tr -d 'A-z')"
}

prepare() {
    chmod a+x bilingwise
    ./bilingwise "$srcdir"/$bilingwise_lang/messages/kdemultimedia/kdenlive.po > /dev/null
    mv "$srcdir"/$bilingwise_lang/messages/kdemultimedia/kdenlive.po.ru-with-en.po "$srcdir"/$inflate_lang/messages/kdemultimedia/kdenlive.po

    ./bilingwise "$srcdir"/$bilingwise_lang/messages/frameworks/kxmlgui5.po > /dev/null
    mv "$srcdir"/$bilingwise_lang/messages/frameworks/kxmlgui5.po.ru-with-en.po "$srcdir"/$inflate_lang/messages/frameworks/kxmlgui5.po

    ./bilingwise "$srcdir"/$bilingwise_lang/messages/frameworks/kconfigwidgets5.po > /dev/null
    mv "$srcdir"/$bilingwise_lang/messages/frameworks/kconfigwidgets5.po.ru-with-en.po "$srcdir"/$inflate_lang/messages/frameworks/kconfigwidgets5.po

    echo -e "add_subdirectory(frameworks)\nadd_subdirectory(kdemultimedia)\n" > "$srcdir"/$inflate_lang/messages/CMakeLists.txt # to avoid building unnesessary directories
}

build() {
    ./scripts/autogen.sh $inflate_lang
    mkdir -p $inflate_lang-build
    cd $inflate_lang-build
    cmake \
      -DCMAKE_BUILD_TYPE=Release \
      -DCMAKE_INSTALL_PREFIX=/usr\
      ../$inflate_lang
    make
}

package() {
    cd $inflate_lang-build
    make DESTDIR="$pkgdir/" install
    mv "$pkgdir"/usr/share/locale/$inflate_lang "$pkgdir"/usr/share/locale/$replace_lang
}
