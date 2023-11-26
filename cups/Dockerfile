ARG BUILD_FROM
FROM $BUILD_FROM

LABEL io.hass.version="1.0" io.hass.type="addon" io.hass.arch="aarch64|amd64"

# Set shell
SHELL ["/bin/bash", "-o", "pipefail", "-c"]

RUN apt-get update \
    && apt-get install -y --no-install-recommends \
        sudo \
        locales \
        cups \
        avahi-daemon \
        libnss-mdns \
        dbus \
        colord \
        printer-driver-all-enforce \
        openprinting-ppds \
        hpijs-ppds \
        hp-ppd  \
        hplip \
        cups-pdf \
        gnupg2 \
        lsb-release \
        nano \
        samba \
        bash-completion \
        procps \
        whois \
        curl \
    && apt-get clean -y \
    && rm -rf /var/lib/apt/lists/* \
    && curl -o ~/driver.deb https://download.brother.com/welcome/dlf005461/dcp135ccupswrapper-1.0.1-1.i386.deb \
    && dpkg -i ~/driver.deb

COPY rootfs /

# Add user and disable sudo password checking
RUN useradd \
  --groups=sudo,lp,lpadmin \
  --create-home \
  --home-dir=/home/print \
  --shell=/bin/bash \
  --password=$(mkpasswd print) \
  print \
&& sed -i '/%sudo[[:space:]]/ s/ALL[[:space:]]*$/NOPASSWD:ALL/' /etc/sudoers

EXPOSE 631

RUN chmod a+x /run.sh

CMD ["/run.sh"]
