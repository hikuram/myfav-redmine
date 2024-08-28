FROM redmine:5

COPY --chown=redmine:redmine ./config/configuration.yml config

RUN set -eux; \
        \
        apt-get update && apt-get install -y --no-install-recommends \
                gcc g++ \
                make \
                nano \
                zstd \
        ; \
        rm -rf /var/lib/apt/lists/*; \
        \
# install plugins
        gosu redmine git clone --depth 1 https://github.com/agileware-jp/redmine_banner.git plugins/redmine_banner; \
        gosu redmine git clone --depth 1 https://github.com/clear-code/redmine_full_text_search.git plugins/full_text_search; \
        gosu redmine git clone --depth 1 https://github.com/onozaty/redmine-view-customize.git plugins/view_customize; \
        gosu redmine git clone --depth 1 https://github.com/haru/redmine_wiki_extensions.git plugins/redmine_wiki_extensions; \
        gosu redmine git clone --depth 1 https://github.com/haru/redmine_theme_changer.git plugins/redmine_theme_changer; \
        gosu redmine git clone --depth 1 https://github.com/hikuram/additionals.git plugins/additionals; \
        gosu redmine git clone --depth 1 https://github.com/AlphaNodes/additional_tags.git plugins/additional_tags; \
        gosu redmine git clone --depth 1 https://github.com/AlphaNodes/redmine_lightbox.git plugins/redmine_lightbox; \
        gosu redmine git clone --depth 1 https://github.com/ryouma-nagare/redmine_jstoolbar_ext.git plugins/redmine_jstoolbar_ext; \
        gosu redmine git clone --depth 1 https://github.com/ryouma-nagare/redmine_jstoolbar_ext_buttons.git plugins/redmine_jstoolbar_ext_buttons; \
        gosu redmine git clone --depth 1 https://github.com/hikuram/redmine_already_read.git plugins/redmine_already_read; \
        gosu redmine git clone --depth 1 https://github.com/nanego/redmine_hide_sidebar.git plugins/redmine_hide_sidebar; \
        gosu redmine git clone --depth 1 https://github.com/taqueci/redmine_local_avatars.git plugins/redmine_local_avatars; \
        gosu redmine git clone --depth 1 https://github.com/hikuram/redmine_wysiwyg_editor.git  plugins/redmine_wysiwyg_editor; \
        gosu redmine git clone --depth 1 https://github.com/HugoHasenbein/redmine_more_previews.git plugins/redmine_more_previews; \
        gosu redmine git clone --depth 1 https://gitlab.com/redmine-plugin-description-sidebar/redmine-plugin-description-sidebar.git plugins/description_sidebar; \
        gosu redmine git clone --depth 1 https://github.com/redmica/redmine_ip_filter.git plugins/redmine_ip_filter; \
        gosu redmine git clone --depth 1 https://www.github.com/sk-ys/redmine_gantt_tweak plugins/redmine_gantt_tweak; \
        gosu redmine curl --location https://github.com/ONLYOFFICE/onlyoffice-redmine/releases/latest/download/onlyoffice_redmine.tar.zst --output onlyoffice_redmine.tar.zst; \
        gosu redmine tar --extract --file onlyoffice_redmine.tar.zst --directory plugins; \
        gosu redmine rm onlyoffice_redmine.tar.zst; \
        \
# install themes
        gosu redmine git clone --depth 1 https://github.com/farend/redmine_theme_farend_basic.git public/themes/farend_basic; \
        gosu redmine git clone -b redmine4.1 --depth 1 https://github.com/farend/redmine_theme_farend_bleuclair.git public/themes/bleuclair; \
        gosu redmine git clone --depth 1 https://github.com/farend/redmine_theme_farend_fancy.git public/themes/farend_fancy; \
        gosu redmine git clone --depth 1 https://github.com/makotokw/redmine-theme-gitmike.git public/themes/gitmike; \
        \
# install gems
        gosu redmine bundle install;
