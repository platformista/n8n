# n8n on Upsun

Deploy [n8n](https://n8n.io/) — the open-source workflow automation tool — on [Upsun](https://upsun.com) in minutes. No infrastructure management, no DevOps overhead, just powerful automation with a drag-and-drop interface.

> **Related article:** [NoCode with Upsun: Supercharge Your Automation with Zero Code](https://devcenter.upsun.com/posts/nocode-n8n/)

## What's included

| Component | Details |
|---|---|
| **Runtime** | Node.js 22 |
| **n8n packages** | `n8n` (latest) + `n8n-nodes-mcp` (MCP community nodes) |
| **Persistent storage** | `.n8n/` (workflows, credentials, DB) and `.cache/` |
| **SMTP** | Pre-configured for Upsun's built-in SMTP relay (port 25) |
| **Task Runners** | Enabled via `N8N_RUNNERS_ENABLED` |
| **HTTPS** | Automatic — HTTP requests are redirected to HTTPS |

## Prerequisites

- An [Upsun account](https://auth.upsun.com/register)
- The [Upsun CLI](https://docs.upsun.com/administration/cli.html) installed
- Git installed locally

## Deploy

### 1. Clone this repository

```bash
git clone <this-repo-url>
cd n8n
```

### 2. Create an Upsun project

```bash
upsun project:create
```

Follow the CLI prompts to set up your project and link it to this repository.

### 3. Push to Upsun

```bash
git add .
git commit -m "Initial n8n deployment"
upsun push
```

After deployment completes, grab your live URL:

```bash
upsun environment:url --primary
```

Open the URL in your browser and create your n8n account.

## Configuration overview

All Upsun configuration lives in [`.upsun/config.yaml`](.upsun/config.yaml).

### Key points

- **Dynamic port binding** — n8n needs to listen on the port assigned by Upsun. Since `variables.env` doesn't support referencing other variables, the `hooks.build` step writes `N8N_PORT=$PORT` and `N8N_LISTEN_ADDRESS=0.0.0.0` into `.environment`, which is sourced at runtime.
- **Mounts** — The `.n8n` mount persists your workflows, credentials, and SQLite database across deployments. The `.cache` mount keeps temporary data out of the read-only filesystem.
- **Request buffering disabled** — Required for n8n's real-time features (webhooks, SSE, streaming).

### Environment variables

You can set additional n8n environment variables via the Upsun CLI:

```bash
upsun variable:create --name env:N8N_ENCRYPTION_KEY --value '<your-key>' --sensitive true
```

See the [n8n environment variables reference](https://docs.n8n.io/hosting/configuration/environment-variables/) for the full list.

## Example use cases

A few real-world automations you can build with n8n:

- 📧 **Email marketing** — Sync new Airtable contacts to Mailchimp
- 💬 **Slack alerts** — Notify your team when a customer submits a form
- 📈 **Social monitoring** — Track mentions and store them in Notion
- 🗂️ **File watchers** — Upload new Dropbox files to Google Drive automatically
- 🤖 **AI agents** — Use MCP nodes (`n8n-nodes-mcp`) to connect AI tools to your workflows

## Resources

- [n8n documentation](https://docs.n8n.io/)
- [Upsun documentation](https://docs.upsun.com/)
- [Upsun CLI reference](https://docs.upsun.com/administration/cli.html)
- [Demo project on GitHub](https://github.com/upsun/demo-n8n)
- [Upsun Discord community](https://discord.gg/upsun)

## License

This project template is provided as-is. n8n itself is licensed under the [Sustainable Use License](https://github.com/n8n-io/n8n/blob/master/LICENSE.md).
