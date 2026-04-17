# version-update-pr-body

A small composite GitHub Action that renders a standardized pull request body for Go version updates.

It uses [Gomplate](https://gomplate.ca/) internally through the Marketplace action [`ammarlakis/action-gomplate`](https://github.com/marketplace/actions/gomplate-template-processor), so consuming workflows only need to pass version values.

## Inputs

- `output-file`: path to write the rendered body
- `go-changed`: whether the Go version changed
- `go-previous-version`: previous Go version
- `go-current-version`: current Go version
- `lint-changed`: whether the lint toolchain changed
- `lint-previous-action-version`: previous `golangci-lint-action` version
- `lint-current-action-version`: current `golangci-lint-action` version
- `lint-previous-version`: previous `golangci-lint` version
- `lint-current-version`: current `golangci-lint` version
- `lint-fix-ran`: whether `make lint-fix` ran after the update

## Example

```yaml
- name: Render version update PR body
  uses: faisal-memon/version-update-pr-body@v1
  with:
    output-file: ${{ runner.temp }}/pr_body.md
    go-changed: ${{ steps.update-go.outputs.changed }}
    go-previous-version: ${{ steps.update-go.outputs.previous-version }}
    go-current-version: ${{ steps.update-go.outputs.current-version }}
    lint-changed: ${{ steps.update-lint.outputs.changed }}
    lint-previous-action-version: ${{ steps.update-lint.outputs.previous-action-version }}
    lint-current-action-version: ${{ steps.update-lint.outputs.current-action-version }}
    lint-previous-version: ${{ steps.update-lint.outputs.previous-lint-version }}
    lint-current-version: ${{ steps.update-lint.outputs.current-lint-version }}
    lint-fix-ran: 'true'
```

The rendered body is intended to be used with `peter-evans/create-pull-request` via `body-path`.
