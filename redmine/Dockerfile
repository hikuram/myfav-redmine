FROM redmine:6

COPY --chown=redmine:redmine ./config/configuration.yml config

RUN set -eux; \
        \
        apt-get update && apt-get install -y --no-install-recommends \
                gcc g++ \
                make \
                nano \
        ; \
        rm -rf /var/lib/apt/lists/*; \
        \
# install plugins
        gosu redmine git clone --depth 1 https://github.com/clear-code/redmine_full_text_search.git plugins/full_text_search; \
        gosu redmine git clone --depth 1 https://github.com/onozaty/redmine-view-customize.git plugins/view_customize; \
        gosu redmine git clone --depth 1 https://github.com/haru/redmine_wiki_extensions.git plugins/redmine_wiki_extensions; \
        gosu redmine git clone --depth 1 https://github.com/haru/redmine_theme_changer.git plugins/redmine_theme_changer; \
        gosu redmine git clone --depth 1 https://github.com/AlphaNodes/redmine_lightbox.git plugins/redmine_lightbox; \
        #aosu redmine git clone --depth 1 https://github.com/ryouma-nagare/redmine_jstoolbar_ext.git plugins/redmine_jstoolbar_ext; \
        #gosu redmine git clone --depth 1 https://github.com/ryouma-nagare/redmine_jstoolbar_ext_buttons.git plugins/redmine_jstoolbar_ext_buttons; \
        gosu redmine git clone --depth 1 https://github.com/taqueci/redmine_local_avatars.git plugins/redmine_local_avatars; \
        gosu redmine git clone --depth 1 https://github.com/taqueci/redmine_wysiwyg_editor.git  plugins/redmine_wysiwyg_editor; \
        #gosu redmine git clone --depth 1 https://github.com/HugoHasenbein/redmine_more_previews.git plugins/redmine_more_previews; \
        \
# install themes
        gosu redmine git clone --depth 1 https://github.com/farend/redmine_theme_farend_basic.git themes/farend_basic; \
        gosu redmine git clone --depth 1 https://github.com/farend/redmine_theme_farend_bleuclair.git themes/bleuclair; \
        gosu redmine git clone --depth 1 https://github.com/farend/redmine_theme_farend_fancy.git themes/farend_fancy; \
        #gosu redmine git clone --depth 1 https://github.com/makotokw/redmine-theme-gitmike.git themes/gitmike; \
        \
# install gems
        gosu redmine bundle install;
