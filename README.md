# 🚀 Get Latest GHCR Tag

Fetches the **latest tag** of a [GitHub Container Registry (GHCR)](https://ghcr.io) image that matches a given identifier, using the GitHub CLI.

Created by **@nickkkostov**

---

## 📦 Usage

```yaml
- name: Get latest GHCR tag
  uses: nickkostov/get-latest-ghcr-tag@v1
  id: latest
  with:
    image: my-container
    image_identifier: prod
    gh_token: ${{ secrets.GITHUB_TOKEN }}
    org: my-org             # optional
    # user: my-username     # optional
```

```yaml
# Use the output
- name: Print latest tag
  run: echo "Latest tag is: ${{ steps.latest.outputs.latest_tag }}"
```

---

## 🔧 Inputs

| Name              | Required | Description                                                                 |
|-------------------|----------|-----------------------------------------------------------------------------|
| `image`           | ✅       | The name of the GHCR container image                                       |
| `image_identifier`| ✅       | A substring used to match relevant tags (e.g., `prod`, `stable`, etc.)     |
| `gh_token`        | ✅       | GitHub token for authentication (use `${{ secrets.GITHUB_TOKEN }}`)        |
| `org`             | ❌       | GitHub organization owning the image (if applicable)                       |
| `user`            | ❌       | GitHub user owning the image (if applicable)                               |

> ❗ You **must provide either** `org` or `user`.

---

## 📤 Outputs

| Name         | Description                             |
|--------------|-----------------------------------------|
| `latest_tag` | The latest tag matching the identifier  |

---

## 🧪 Example

If your GHCR repo has the following tags:
```
latest, dev-2024.01, prod-2024.03, prod-2024.05
```

With:
```yaml
image_identifier: prod
```

Output will be:
```
prod-2024.05
```

---

## 🔐 Permissions

Ensure your GitHub token has at least `read:packages` permission to access GHCR metadata.

---

## 🛠 How it works

1. Installs `gh` and `jq` if needed.
2. Authenticates using the provided token.
3. Queries the `orgs/` or `users/` endpoint for container versions.
4. Filters tags by substring match and returns the latest (sorted semantically).

---

## 🖼 Branding

| Icon | Color |
|------|--------|
| `tag` | `red` |

---

## 📜 License

MIT – feel free to use and contribute!
