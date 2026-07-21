FROM postgres:17-trixie

RUN set -eux; \
    apt-get update; \
    apt-get install -y --no-install-recommends \
        ca-certificates \
        wget \
    ; \
    \
    . /etc/os-release; \
    distro="$(printf '%s' "$ID" | tr 'A-Z' 'a-z')"; \
    codename="$VERSION_CODENAME"; \
    workdir="$(mktemp -d)"; \
    cd "$workdir"; \
    \
    wget "https://apache.jfrog.io/artifactory/arrow/${distro}/apache-arrow-apt-source-latest-${codename}.deb"; \
    apt-get install -y -V "./apache-arrow-apt-source-latest-${codename}.deb"; \
    apt-get update; \
    wget "https://packages.groonga.org/debian/groonga-apt-source-latest-${codename}.deb"; \
    apt-get install -y -V "./groonga-apt-source-latest-${codename}.deb"; \
    apt-get update; \
    apt-get install -y -V --no-install-recommends \
        postgresql-17-pgdg-pgroonga \
        groonga-tokenizer-mecab \
    ; \
    \
    cd /; \
    rm -rf "$workdir"; \
    apt-get purge -y --auto-remove wget; \
    rm -rf /var/lib/apt/lists/* /tmp/* /var/tmp/*
