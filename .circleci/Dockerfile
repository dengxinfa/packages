FROM debian:10


# Configuration version history
# v1.0   - Initial version by Etienne Champetier
# v1.0.1 - Run as non-root, add unzip, xz-utils
# v1.0.2 - Add bzr
# v1.0.3 - Verify usign signatures
# v1.0.4 - Add support for Python3
# v1.0.5 - Add 19.07 public keys, verify keys
# v1.0.6 - Add 21.02 public keys, update Debian image to version 10, add rsync
# v1.0.7 - Add 22.03 public keys, 18.06 v2 gpg key, 18.06 usign key

RUN apt update && apt install -y \
build-essential \
bzr \
curl \
jq \
gawk \
gettext \
git \
libncurses5-dev \
libssl-dev \
python \
python3 \
signify-openbsd \
subversion \
rsync \
time \
unzip \
wget \
xz-utils \
zlib1g-dev \
&& rm -rf /var/lib/apt/lists/*

RUN useradd -c "OpenWrt Builder" -m -d /home/build -s /bin/bash build
USER build
ENV HOME /home/build

# OpenWrt Build System (PGP key for unattended snapshot builds)
RUN curl 'https://git.openwrt.org/?p=keyring.git;a=blob_plain;f=gpg/626471F1.asc' | gpg --import \
 && gpg --fingerprint --with-colons '<pgpsign-snapshots@openwrt.org>' | grep '^fpr:::::::::54CC74307A2C6DC9CE618269CD84BCED626471F1:$' \
 && echo '54CC74307A2C6DC9CE618269CD84BCED626471F1:6:' | gpg --import-ownertrust

# OpenWrt Build System (PGP key for 17.01 "Reboot" release builds)
RUN curl 'https://git.openwrt.org/?p=keyring.git;a=blob_plain;f=gpg/D52BBB6B.asc' | gpg --import \
 && gpg --fingerprint --with-colons '<pgpsign-17.01@openwrt.org>' | grep '^fpr:::::::::B09BE781AE8A0CD4702FDCD3833C6010D52BBB6B:$' \
 && echo 'B09BE781AE8A0CD4702FDCD3833C6010D52BBB6B:6:' | gpg --import-ownertrust

# OpenWrt Release Builder (18.06 Signing Key)
RUN curl 'https://git.openwrt.org/?p=keyring.git;a=blob_plain;f=gpg/17E1CE16.asc' | gpg --import \
 && gpg --fingerprint --with-colons '<openwrt-devel@lists.openwrt.org>' | grep '^fpr:::::::::6768C55E79B032D77A28DA5F0F20257417E1CE16:$' \
 && echo '6768C55E79B032D77A28DA5F0F20257417E1CE16:6:' | gpg --import-ownertrust

# OpenWrt Build System (PGP key for 18.06 release builds)
RUN curl 'https://git.openwrt.org/?p=keyring.git;a=blob_plain;f=gpg/15807931.asc' | gpg --import \
 && gpg --fingerprint --with-colons '<pgpsign-18.06@openwrt.org>' | grep '^fpr:::::::::AD0507363D2BCE9C9E36CEC4FBCB78F015807931:$' \
 && echo 'AD0507363D2BCE9C9E36CEC4FBCB78F015807931:6:' | gpg --import-ownertrust

# OpenWrt Build System (PGP key for 19.07 release builds)
RUN curl 'https://git.openwrt.org/?p=keyring.git;a=blob_plain;f=gpg/2074BE7A.asc' | gpg --import \
 && gpg --fingerprint --with-colons '<pgpsign-19.07@openwrt.org>' | grep '^fpr:::::::::D9C6901F45C9B86858687DFF28A39BC32074BE7A:$' \
 && echo 'D9C6901F45C9B86858687DFF28A39BC32074BE7A:6:' | gpg --import-ownertrust

# OpenWrt Build System (PGP key for 21.02 release builds)
RUN curl 'https://git.openwrt.org/?p=keyring.git;a=blob_plain;f=gpg/88CA59E8.asc' | gpg --import \
 && gpg --fingerprint --with-colons '<pgpsign-21.02@openwrt.org>' | grep '^fpr:::::::::667205E379BAF348863A5C6688CA59E88F681580:$' \
 && echo '667205E379BAF348863A5C6688CA59E88F681580:6:' | gpg --import-ownertrust

# OpenWrt Build System (GnuPGP key for 22.03 release builds)
RUN curl 'https://git.openwrt.org/?p=keyring.git;a=blob_plain;f=gpg/CD54E82DADB3684D.asc' | gpg --import \
 && gpg --fingerprint --with-colons '<pgpsign-22.03@openwrt.org>' | grep '^fpr:::::::::BF856781A01293C8409ABE72CD54E82DADB3684D:$' \
 && echo 'BF856781A01293C8409ABE72CD54E82DADB3684D:6:' | gpg --import-ownertrust

# untrusted comment: Public usign key for unattended snapshot builds
RUN curl 'https://git.openwrt.org/?p=keyring.git;a=blob_plain;f=usign/b5043e70f9a75cde' --create-dirs -o /home/build/usign/b5043e70f9a75cde \
 && echo 'd7ac10f9ed1b38033855f3d27c9327d558444fca804c685b17d9dcfb0648228f */home/build/usign/b5043e70f9a75cde' | sha256sum --check

# untrusted comment: Public usign key for 18.06 release builds
RUN curl 'https://git.openwrt.org/?p=keyring.git;a=blob_plain;f=usign/1035ac73cc4e59e3' --create-dirs -o /home/build/usign/1035ac73cc4e59e3 \
 && echo '8dc2e7f5c4e634437e6641f4df77a18bf59f0c8e9016c8ba4be5d4a0111e68c2 */home/build/usign/1035ac73cc4e59e3' | sha256sum --check

# untrusted comment: Public usign key for 19.07 release builds
RUN curl 'https://git.openwrt.org/?p=keyring.git;a=blob_plain;f=usign/f94b9dd6febac963' --create-dirs -o /home/build/usign/f94b9dd6febac963 \
 && echo 'b1d09457cfbc36fccfe18382d65c54a2ade3e7fd3902da490a53aa517b512755 */home/build/usign/f94b9dd6febac963' | sha256sum --check

# untrusted comment: Public usign key for 21.02 release builds
RUN curl 'https://git.openwrt.org/?p=keyring.git;a=blob_plain;f=usign/2f8b0b98e08306bf' --create-dirs -o /home/build/usign/2f8b0b98e08306bf \
 && echo 'd102bdd75421c62490b97f520f9db06aadb44ad408b244755d26e96ea5cd3b7f */home/build/usign/2f8b0b98e08306bf' | sha256sum --check

# untrusted comment: Public usign key for 22.03 release builds
RUN curl 'https://git.openwrt.org/?p=keyring.git;a=blob_plain;f=usign/4d017e6f1ed5d616' --create-dirs -o /home/build/usign/4d017e6f1ed5d616 \
 && echo 'f3c5fdf447d7c2743442e68077d60acc7c3e91754849e1f4b6be837b4204b7e2 */home/build/usign/4d017e6f1ed5d616' | sha256sum --check
