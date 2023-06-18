# Arch-Spotify-Fix
PKGBUILD tries to get a spotify version that doesn't exist and shows this error: 
![image](https://github.com/Noft1337/Arch-Spotify-1.2.11.916.geb595a67-Fix/assets/101609620/5bfaa393-7e17-4169-ab40-168bdb5c0437)


# Relevancy
This applies to `Spotify v1.2.11.916.geb595a67`, 
The PKGBUILD file tries to get `Spotify v1.2.9.743.g85d9593d`
So if you are experiencing issues with a different version this would not work. 
**However**, I will add a quick fix/workaround for other versions in the bottom of the `README`

# Fix
to fix this issue first run this command to update the `PGP Public Key`:
```bash
curl -sS https://download.spotify.com/debian/pubkey_7A3A762FAFD4A51F.gpg | gpg --import -
```
and then copy `PKGBUILD` to your cloned `AUR Spotify` and run:
`makepkg -Si && makepkg -i` 
Spotify will now be installed.

# Workaround 
If you're experiencing this with a different spotify version then follow these steps and you should be good to go.
verify which spotify version actually exists in [here](http://repository.spotify.com/pool/non-free/s/spotify-client/)
and then get the version of it and change it in `PKGBUILD` **(I've also added little comments to show where to change the parameters)**
for example, if version `1.2.3.4.5_abcdefg` exists then change `pkgver` and `_commit` accordingly like this:
```bash
#   _amd64
pkgname=spotify
# Change Version
pkgver='1.2.3.4.5'
epoch=1
# Change Commit 
_commit=abcdefg
pkgrel=1
pkgdesc='A proprietary music streaming service'
arch=('x86_64')
license=('custom')
url='https://www.spotify.com'
```
and you will also need to disable the `checksum verification`:
```bash
source=('spotify.protocol'
        'LICENSE'
        "${pkgname}-${pkgver}-${_commit}-x86_64.deb::http://repository.spotify.com/pool/non-free/s/spotify-client/spotify-client_${pkgver}.${_commit}_amd64.deb"
        # GPG signature check
        "${pkgname}-${pkgver}-${pkgrel}-Release::http://repository.spotify.com/dists/testing/Release"
        "${pkgname}-${pkgver}-${pkgrel}-Release.sig::http://repository.spotify.com/dists/testing/Release.gpg"
        "${pkgname}-${pkgver}-${pkgrel}-x86_64-Packages::http://repository.spotify.com/dists/testing/non-free/binary-amd64/Packages")
sha512sums=('999abe46766a4101e27477f5c9f69394a4bb5c097e2e048ec2c6cb93dfa1743eb436bde3768af6ba1b90eaac78ea8589d82e621f9cbe7d9ab3f41acee6e8ca20'
            '2e16f7c7b09e9ecefaa11ab38eb7a792c62ae6f33d95ab1ff46d68995316324d8c5287b0d9ce142d1cf15158e61f594e930260abb8155467af8bc25779960615'
            # Change this to SKIP if you don't have the new checksum.
            # |
            # V
            'SKIP'
            'SKIP'
            'SKIP'
            'SKIP')
```
and then run
```bash
makepkg -Si && makepkg -i
```
