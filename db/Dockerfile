FROM postgres:14

RUN set -eux; \
        \
        apt-get update && apt-get install -y --no-install-recommends \
                ca-certificates \
                wget \
        ; \
        rm -rf /var/lib/apt/lists/*; \
        \
        mkdir pgroonga-work; \
        cd pgroonga-work; \
        \
        wget https://packages.groonga.org/debian/groonga-apt-source-latest-bullseye.deb; \
        apt-get install -y ./groonga-apt-source-latest-bullseye.deb; \
        apt-get update && apt-get install -y --no-install-recommends \
                postgresql-14-pgdg-pgroonga \
                groonga-tokenizer-mecab \
        ; \
        rm -rf /var/lib/apt/lists/*; \
        \
        cd ..; \
        rm -rf pgroonga-work;
