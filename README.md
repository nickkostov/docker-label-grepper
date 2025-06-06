# 🔖 docker-label-grepper

**Get the latest Docker image tag from GitHub Container Registry (GHCR) that matches a specific label or pattern**  
This GitHub Action uses the GitHub CLI (`gh`) to fetch and sort tags, helping you automatically detect your latest `prod`, `stable`, or other tagged builds.

---

## 📥 Inputs

| Name              | Description                                                         | Required |
|-------------------|---------------------------------------------------------------------|----------|
| `identity`         | GitHub username or organization that owns the image (`my-org`)      | ✅ Yes    |
| `image`            | GHCR image name (`my-app`)                                          | ✅ Yes    |
| `image_identifier` | A label or substring to match tags against (e.g., `prod`, `stable`) | ✅ Yes    |

---

## 📤 Outputs

| Name         | Description                                  |
|--------------|----------------------------------------------|
| `latest_tag` | The most recent tag matching your identifier |

---

## 🚀 Example Usage

```yaml
jobs:
  get-latest-tag:
    runs-on: ubuntu-latest
    steps:
      - uses: your-org/docker-label-grepper@v1
        id: ghcr
        with:
          identity: your-org
          image: your-image
          image_identifier: prod

      - name: Use the tag
        run: echo "Latest prod tag is: ${{ steps.ghcr.outputs.latest_tag }}"
```

---

## ✅ Requirements

- Uses [GitHub CLI](https://cli.github.com/) (`gh`) and `jq`
- Requires no secrets — uses the built-in `GITHUB_TOKEN`

---

## 🧠 How It Works

1. Fetches all versions of the GHCR image via the GitHub API
2. Filters tags that match your `image_identifier`
3. Sorts them semantically (e.g., `1.0.0-prod`, `1.1.0-prod`)
4. Returns the latest one as `latest_tag`

---

## 📦 Example Image Structure

This action targets images hosted at:

```
ghcr.io/<identity>/<image>
```

For example:

```
ghcr.io/my-org/my-api-service
```

---

## 📚 Full Example With Docker Deployment

```yaml
name: Deploy using latest prod tag

on:
  workflow_dispatch:

jobs:
  deploy:
    runs-on: ubuntu-latest
    steps:
      - uses: your-org/docker-label-grepper@v1
        id: tagger
        with:
          identity: your-org
          image: my-app
          image_identifier: prod

      - name: Pull Docker image
        run: |
          docker pull ghcr.io/your-org/my-app:${{ steps.tagger.outputs.latest_tag }}

      - name: Deploy container
        run: |
          docker run -d ghcr.io/your-org/my-app:${{ steps.tagger.outputs.latest_tag }}
```

---

## 🧑‍💻 Maintainer

Made with ❤️ by [your-org](https://github.com/your-org)

---

## 📄 License

[MIT](LICENSE)
