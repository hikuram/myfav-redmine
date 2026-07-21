# Redmine 7 Docker Compose Environment

A Docker Compose setup for running Redmine 7 with PostgreSQL-based full-text search, attachment text extraction, Markdown editing and export tools, and selected themes.

## Stack

| Component | Configuration |
| --- | --- |
| Redmine | Official `redmine:7` image with additional plugins, themes, and Pandoc 3.10 |
| Database | PostgreSQL 17 with PGroonga and MeCab tokenization |
| Text extraction | ChupaText server with an outbound proxy |
| Time zone | `Asia/Tokyo` |
| Web port | `3000` on the Docker host |

The default configuration assumes a Linux host. The Redmine image currently supports `amd64` builds because it installs a pinned amd64 Pandoc package.

## Included Redmine plugins

- [Full Text Search](https://github.com/clear-code/redmine_full_text_search)
- [View Customize](https://github.com/onozaty/redmine-view-customize)
- [Wiki Extensions](https://github.com/haru/redmine_wiki_extensions) (`develop` branch)
- [Theme Changer](https://github.com/haru/redmine_theme_changer)
- [Lightbox](https://github.com/AlphaNodes/redmine_lightbox)
- [Bell Notifications](https://github.com/hikuram/redmine_bell_notifications) (fork)
- [Markdown Export](https://github.com/hikuram/redmine_markdown_export) (fork)
- [Mermaid SVG](https://github.com/hikuram/redmine_mermaid_svg) (fork)
- [Monaco Editor](https://github.com/hikuram/redmine_monaco_editor) (fork)

## Included themes

- [Farend Basic](https://github.com/farend/redmine_theme_farend_basic)
- [Farend Bleuclair](https://github.com/farend/redmine_theme_farend_bleuclair)
- [Farend Fancy](https://github.com/farend/redmine_theme_farend_fancy)

## Quick start

1. Create the local secret files. They are ignored by Git and mounted only into the services that require them.

   ```bash
   umask 077
   mkdir -p secrets
   openssl rand -base64 36 > secrets/postgres_password
   openssl rand -hex 64 > secrets/redmine_secret_key_base
   ```

2. Adjust the host paths used for attachments and logs if necessary:

   - `/srv/redmine/files`
   - `/var/log/redmine`
   - `/var/log/redmine-chupa-text`

3. Configure outgoing email in `redmine/config/configuration.yml`.

4. Build and start the services:

   ```bash
   docker compose up -d --build
   ```

5. Open <http://localhost:3000> and complete the Redmine setup.

6. In the Full Text Search plugin settings, set the ChupaText endpoint to:

   ```text
   http://chupa-text:3000/extraction.json
   ```

## Secrets

`compose.yaml` uses two file-backed Docker Compose secrets:

| Secret | Used by | Purpose |
| --- | --- | --- |
| `postgres_password` | PostgreSQL and Redmine | Shared database password |
| `redmine_secret_key_base` | Redmine | Stable Rails session and signing secret |

The source files remain ordinary files on the Docker host. Keep the `secrets` directory out of backups or repositories that are not intended to contain credentials, restrict its permissions, and preserve `redmine_secret_key_base` across container recreation.

The PostgreSQL password is applied when an empty database volume is initialized. Replacing `secrets/postgres_password` does not change the password stored in an existing PostgreSQL cluster; rotate the database role password before restarting Redmine with a new secret.

### Migrating an existing database volume

If `dbdata` was initialized with the previous hard-coded password, update the existing PostgreSQL role before starting Redmine with the new secret:

```bash
docker compose up -d --build db
docker compose exec db psql -U redmineuser -d postgres
```

At the `psql` prompt, run `\password redmineuser` and enter the exact value stored in `secrets/postgres_password`, then exit with `\q`. Start the complete stack afterward:

```bash
docker compose up -d --build
```

Adding `redmine_secret_key_base` to an existing installation invalidates current browser sessions once, but keeps future sessions stable across container recreation.

For an SMTP password, mount an additional secret into the Redmine service and read it from `configuration.yml` through ERB, for example:

```yaml
password: <%= File.read('/run/secrets/smtp_password').strip %>
```

## Persistent data

- PostgreSQL data is stored in the Docker named volume `dbdata`.
- Redmine attachments and service logs are bind-mounted to the host paths listed above.
- Secret source files are stored under the local `secrets` directory and are not included in the image.

Backups, log rotation, TLS termination, and reverse-proxy configuration are not included. Configure them separately for production use.

## Rebuilding and updating

Run the following command after changing the Dockerfiles or configuration:

```bash
docker compose up -d --build
```

Most plugins, themes, PGroonga repository packages, and ChupaText images are retrieved from their current upstream branches or tags during the build. Rebuilding at a later date may therefore install newer versions. Pandoc is the main explicitly pinned component.

## License

The Compose file and Dockerfiles are provided under the MIT License. See [LICENSE](LICENSE).

Redmine, `configuration.yml`, plugins, themes, container images, and other third-party components remain subject to their respective licenses.
