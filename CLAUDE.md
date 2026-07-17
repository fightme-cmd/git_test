# George's cloud workbench

This repo is a scratch workspace. George's real projects live on his self-hosted
Gitea at `https://git.george1.dev` and deploy via Coolify on his VPS. When George
asks for changes to one of those projects (e.g. **Beug**), work on it like this:

## Workflow: talk → change → live

1. **Clone from Gitea** (credentials are preconfigured by the environment setup
   script — plain HTTPS git just works):

   ```bash
   git clone https://git.george1.dev/george/Beug.git
   cd Beug
   ```

2. **Edit normally.** Run the project's own tests/build (`npm test`,
   `npm run build`) before pushing.

3. **Push to `main`:**

   ```bash
   git push origin main
   ```

4. **Deploy is automatic.** A Gitea webhook triggers a Coolify rebuild on every
   push to `main`. Build takes ~1.5–2 minutes.

5. **Verify the live site** (allowed by the network policy):

   ```bash
   curl -s -o /dev/null -w '%{http_code}\n' https://beug.george1.dev/
   ```

   To prove the new code is actually served, compare a content hash of a built
   file against the live one rather than trusting the HTTP status alone.

## Known projects on Gitea

| Repo | Live URL |
|---|---|
| `george/Beug` | https://beug.george1.dev |
| `george/floatingflows` | https://floatingflows.george1.dev |
| `george/subspeak-media` | https://subspeak-media.george1.dev |

## Rules

- Never force-push. If `main` moved, rebase your commit onto `origin/main`.
- Do NOT use the Gitea MCP connector for file edits — clone and use git.
- SSH to the VPS is not available from cloud sessions (Tailscale-only);
  everything needed (git + live-site checks) works over HTTPS.
- Only `*.george1.dev` and standard package registries are reachable from this
  environment's network policy.
