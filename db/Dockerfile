FROM postgres:15-bookworm

RUN set -eux; \
        \
        apt update && apt install -y --no-install-recommends \
                ca-certificates \
                lsb-release \
                wget \
        ; \
        rm -rf /var/lib/apt/lists/*; \
        \
        mkdir pgroonga-work; \
        cd pgroonga-work; \
        \
        wget https://apache.jfrog.io/artifactory/arrow/$(lsb_release --id --short | tr 'A-Z' 'a-z')/apache-arrow-apt-source-latest-$(lsb_release --codename --short).deb; \
        apt install -y -V ./apache-arrow-apt-source-latest-$(lsb_release --codename --short).deb; \
        apt update; \
        wget https://packages.groonga.org/debian/groonga-apt-source-latest-$(lsb_release --codename --short).deb; \
        apt install -y -V ./groonga-apt-source-latest-$(lsb_release --codename --short).deb; \
        apt update && apt install -y -V \
                postgresql-15-pgdg-pgroonga \
                groonga-tokenizer-mecab \
        ; \
        rm -rf /var/lib/apt/lists/*; \
        \
        cd ..; \
        rm -rf pgroonga-work;
