# Check Markdown

An action using [markdown-link-check](https://github.com/tcort/markdown-link-check) to check that all links
in markdown (`*.md`) files are functional.

## How to use

An example on how to use this action to check all `*.md` files in a repository is shown below.

```yaml
  check-doc-links:
    name: Check links in markdown
    runs-on: ubuntu-latest

    steps:
      - uses: actions/checkout@v4
      - uses: eclipse-kuksa/kuksa-actions/check-markdown@4

```


