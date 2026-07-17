FROM redmine:7

ARG PANDOC_VERSION=3.10
ARG PANDOC_DEB=pandoc-3.10-1-amd64.deb
ARG PANDOC_SHA256=d502599878eb29af3ae5f0cb5d559134df96534125d452c7a0674a5bad2c5ecf

# Use explicit copied assets instead of the plugin's runtime symlink.
ENV REDMINE_MONACO_SKIP_ASSET_LINK=1

COPY --chown=redmine:redmine ./config/configuration.yml config

RUN set -eux; \
    apt-get update; \
    apt-get install -y --no-install-recommends \
        ca-certificates \
        wget \
        git \
        gcc \
        g++ \
        make \
        nano-tiny \
    ; \
    \
    # Install pandoc from the pinned upstream .deb. \
    wget -O /tmp/pandoc.deb "https://github.com/jgm/pandoc/releases/download/${PANDOC_VERSION}/${PANDOC_DEB}"; \
    echo "${PANDOC_SHA256}  /tmp/pandoc.deb" | sha256sum -c -; \
    apt-get install -y --no-install-recommends /tmp/pandoc.deb; \
    rm -f /tmp/pandoc.deb; \
    pandoc --version; \
    \
    # Install Redmine plugins. \
    gosu redmine git clone --depth 1 https://github.com/clear-code/redmine_full_text_search.git plugins/full_text_search; \
    gosu redmine git clone --depth 1 https://github.com/onozaty/redmine-view-customize.git plugins/view_customize; \
    gosu redmine git clone -b develop --depth 1 https://github.com/haru/redmine_wiki_extensions.git plugins/redmine_wiki_extensions; \
    gosu redmine git clone --depth 1 https://github.com/haru/redmine_theme_changer.git plugins/redmine_theme_changer; \
    gosu redmine git clone --depth 1 https://github.com/AlphaNodes/redmine_lightbox.git plugins/redmine_lightbox; \
    gosu redmine git clone --depth 1 https://github.com/hikuram/redmine_bell_notifications.git plugins/redmine_bell_notifications; \
    gosu redmine git clone --depth 1 https://github.com/hikuram/redmine_markdown_export.git plugins/redmine_markdown_export; \
    gosu redmine git clone --depth 1 https://github.com/hikuram/redmine_mermaid_svg.git plugins/redmine_mermaid_svg; \
    gosu redmine git clone --depth 1 https://github.com/hikuram/redmine_monaco_editor.git plugins/redmine_monaco_editor; \
    \
    # Publish only Monaco assets needed at plain, non-fingerprinted paths. \
    rm -rf public/monaco_assets; \
    mkdir -p public/monaco_assets/textgrid; \
    cp -a plugins/redmine_monaco_editor/public_dist/vs public/monaco_assets/vs; \
    cp -a plugins/redmine_monaco_editor/public_dist/textgrid/src public/monaco_assets/textgrid/src; \
    cp -a plugins/redmine_monaco_editor/public_dist/textgrid/styles public/monaco_assets/textgrid/styles; \
    rm -rf plugins/redmine_monaco_editor/public_dist; \
    \
    # Install themes. \
    gosu redmine git clone --depth 1 https://github.com/farend/redmine_theme_farend_basic.git themes/farend_basic; \
    gosu redmine git clone --depth 1 https://github.com/farend/redmine_theme_farend_bleuclair.git themes/bleuclair; \
    gosu redmine git clone --depth 1 https://github.com/farend/redmine_theme_farend_fancy.git themes/farend_fancy; \
    \
    # Remove build-only metadata and install plugin gems. \
    find plugins themes -type d -name .git -prune -exec rm -rf '{}' +; \
    gosu redmine bundle config set without 'development test'; \
    gosu redmine bundle install; \
    \
    # Remove build tools and package caches from the final image. \
    apt-get purge -y --auto-remove wget gcc g++ make; \
    rm -rf \
        /var/lib/apt/lists/* \
        /tmp/* \
        /var/tmp/* \
        /usr/local/bundle/cache \
        /home/redmine/.bundle/cache
