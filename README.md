# action-repository-dispatch

A GitHub Actions action that sends a [`repository_dispatch`](https://docs.github.com/en/rest/repos/repos#create-a-repository-dispatch-event) event to a target repository.

## Usage

```yaml
- uses: sinofseven/action-repository-dispatch@v1
  with:
    target_repo: org/target-repo
    event_type: my-event
    token: ${{ secrets.PAT }}
```

## Inputs

| Name | Required | Default | Description |
|------|----------|---------|-------------|
| `target_repo` | ✅ | — | Target repository to dispatch to (e.g. `org/homebrew-tap`) |
| `event_type` | ✅ | — | The `event_type` for the `repository_dispatch` event |
| `token` | ✅ | — | PAT with Contents (Read and write) permission on the target repository |
| `payload` | — | `{}` | JSON object string to set as `client_payload` |

### Token permissions

When using a fine-grained PAT, the following permission is required on the target repository:

- **Repository permissions > Contents > Read and write**

## Examples

### Passing a payload

```yaml
- uses: sinofseven/action-repository-dispatch@v1.0.0
  with:
    target_repo: org/target-repo
    event_type: deploy
    token: ${{ secrets.PAT }}
    payload: '{"env": "production", "sha": "${{ github.sha }}"}'
```

In the target workflow, the payload is available via `github.event.client_payload`:

```yaml
on:
  repository_dispatch:
    types: [deploy]

jobs:
  deploy:
    runs-on: ubuntu-latest
    steps:
      - run: echo "env=${{ github.event.client_payload.env }}"
```

## License

[MIT](LICENSE)
