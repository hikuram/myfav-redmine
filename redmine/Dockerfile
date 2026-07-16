FROM redmine:7

COPY --chown=redmine:redmine ./config/configuration.yml config

ARG PANDOC_VERSION=3.10
ARG PANDOC_DEB=pandoc-3.10-1-amd64.deb
ARG PANDOC_SHA256=d502599878eb29af3ae5f0cb5d559134df96534125d452c7a0674a5bad2c5ecf

RUN set -eux; \
        \
        apt-get update && apt-get install -y --no-install-recommends \
                ca-certificates \
                wget \
                gcc g++ \
                make \
                nano \
        ; \
        \
# install pandoc
        wget -O /tmp/pandoc.deb "https://github.com/jgm/pandoc/releases/download/${PANDOC_VERSION}/${PANDOC_DEB}"; \
        echo "${PANDOC_SHA256}  /tmp/pandoc.deb" | sha256sum -c -; \
        apt-get install -y --no-install-recommends /tmp/pandoc.deb; \
        rm -f /tmp/pandoc.deb; \
        pandoc --version; \
        rm -rf /var/lib/apt/lists/*; \
        \
# install plugins
        gosu redmine git clone --depth 1 https://github.com/clear-code/redmine_full_text_search.git plugins/full_text_search; \
        gosu redmine git clone --depth 1 https://github.com/onozaty/redmine-view-customize.git plugins/view_customize; \
        gosu redmine git clone --depth 1 https://github.com/haru/redmine_wiki_extensions.git plugins/redmine_wiki_extensions; \
        gosu redmine git clone --depth 1 https://github.com/haru/redmine_theme_changer.git plugins/redmine_theme_changer; \
        gosu redmine git clone --depth 1 https://github.com/AlphaNodes/redmine_lightbox.git plugins/redmine_lightbox; \
        gosu redmine git clone --depth 1 https://github.com/hikuram/redmine_bell_notifications.git  plugins/redmine_bell_notifications; \
        gosu redmine git clone --depth 1 https://github.com/hikuram/redmine_markdown_export.git  plugins/redmine_markdown_export; \
        gosu redmine git clone --depth 1 https://github.com/hikuram/redmine_mermaid_svg.git  plugins/redmine_mermaid_svg; \
        gosu redmine git clone --depth 1 https://github.com/hikuram/redmine_monaco_editor.git  plugins/redmine_monaco_editor; \
        \
# install themes
        gosu redmine git clone --depth 1 https://github.com/farend/redmine_theme_farend_basic.git themes/farend_basic; \
        gosu redmine git clone --depth 1 https://github.com/farend/redmine_theme_farend_bleuclair.git themes/bleuclair; \
        gosu redmine git clone --depth 1 https://github.com/farend/redmine_theme_farend_fancy.git themes/farend_fancy; \
        #gosu redmine git clone --depth 1 https://github.com/makotokw/redmine-theme-gitmike.git themes/gitmike; \
        \
# install gems
        gosu redmine bundle install;
