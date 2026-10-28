# ZubZet for AI agents

You were sent here because you are about to work on a project built on ZubZet, an opinionated PHP framework (Composer package `zubzet/framework`). This page is only a signpost: the documentation and a complete agent skill ship inside the framework package, so the right version of both is already in the project's `vendor` folder. Read this page once, then continue from the files it names. Do not write code before step 2.

## 1. Find out where you are

Look for a `composer.json` that requires `zubzet/framework` and for the folder `vendor/zubzet/framework/`.

- **Both exist**: an installed project. Continue with step 2.
- **The project files exist but `vendor/` is missing**: dependencies are not installed yet. Run `./project start` (step 4); it installs them inside the Docker container. Then continue with step 2.
- **An empty folder, or no ZubZet files at all**: a new project. The installation guide at <https://zubzet.com/docs/latest/setup/installation/> comes down to one command that needs nothing but Docker on the host: `bash <(curl -fsSL https://zubzet.com/install.sh) my-project`. Tell the user before you run it, run it, start the project (step 4), then continue with step 2. `https://zubzet.com/install.sh` is a redirect to the script inside the documentation of the latest stable release, `https://zubzet.com/docs/latest/setup/install.sh`; the script is versioned with the framework, so when the user wants a different version, fetch it from that version's docs instead, `https://zubzet.com/docs/<version>/setup/install.sh`, for example `edge` for the newest pre-release.

**Docker is required** for every ZubZet project. The application, the database, the mail catcher and the test runner all run in containers started by the project's own `./project` script. If Docker is not available where you run, say so and stop; do not try to install PHP or MariaDB on the host instead. Tell the user to open an issue informing the maintainers about the system, so they can decide to support it.

## 2. Read the agent skill

`vendor/zubzet/framework/skills/zubzet/SKILL.md` holds the architecture, design and quality rules for ZubZet projects: what goes into a controller, a model and a view, how forms, migrations, seeds and tests are done, the coding style, and which further files to read for the area you are about to touch. It is written for you. Read it completely before you change or add any code, and follow the reading order it gives.

Claude Code loads it when the project links it as `.claude/skills/zubzet`. Check whether that link exists. Where it is missing, offer the user to create it; the framework does not create it itself, because a `.claude` folder is one tool's convention. If you use a different folder to load project skills, use that and tell the user.

```bash
mkdir -p .claude/skills
ln -s ../../vendor/zubzet/framework/skills/zubzet .claude/skills/zubzet
```

Use exactly this relative target: it follows the installed framework version, so a `composer update` updates the skill too. Agents other than Claude Code read `SKILL.md` and its `reference/` and `code-style/` files directly; a line in the project's `AGENTS.md` pointing at the skill keeps that discoverable.

## 3. Use the bundled documentation

`vendor/zubzet/framework/docs/` is the full documentation as Markdown, with `vendor/zubzet/framework/mkdocs.yml` as its table of contents (read the `nav` section first). The same content is hosted at <https://zubzet.com/docs>, but the vendor copy matches the installed version exactly, so prefer it. When the docs stop, the source is in `vendor/zubzet/framework/src/`; grep for the method name. Look APIs up instead of guessing, and never rebuild something the framework already provides (login, permissions, forms, migrations, the admin panel, email, logging: the skill lists them).

## 4. Run the project

Everything goes through the `project` script in the project root, which drives the Docker stack:

| Command | What it does |
| --- | --- |
| `./project start` | builds the image, starts the stack, installs Composer dependencies, seeds the database, prints the startup info |
| `./project stop` | stops the stack and removes its volumes |
| `./project shell` | a shell inside the application container |
| `./project tests` | runs the Cypress end to end suite headless |
| `./project db:seed` | rebuilds the database from migrations and seeds |

The application answers at <http://localhost:8080>, phpMyAdmin at <http://localhost:8081>, the mail catcher at <http://localhost:3300>. The seed creates demo accounts (`admin@zubzet.com`, `support@zubzet.com`, `customer@zubzet.com`; passwords in `app/Database/seed/accounts/`). Projects wrap the same stack in `npm run start`, `npm run stop` and `npm run tests`; the project's `README.md` usually says which.

## 5. Project specifics win

An `AGENTS.md` or `CLAUDE.md` in the project root holds what cannot be inferred: the customer, the deployment target, odd constraints. Where it differs from the skill, the project file wins. If there is none, offer to write a short one once you know the project, as the skill describes.

## If you cannot reach the sources

When the project is not installed and cannot be installed where you are (no Docker), the same files are public: the skill at <https://github.com/zubzet/framework/tree/develop/skills/zubzet>, the documentation at <https://zubzet.com/docs>, the framework source at <https://github.com/zubzet/framework>. Use them to understand the project, and tell the user that running or testing it needs Docker.

This page lives at <https://zubzet.com/agent.md>. The skill and the docs it points to are versioned with the framework; this page is not, so it stays short on purpose.
