# codeql-action

This action runs GitHub's CodeQL analysis (https://codeql.github.com/) on the provided input language.
It can be used in a matrix-strategy job to perform CodeQL on multiple language at once.
This action essentially abstracts the example CodeQL workflow into a composite action to simplify workflows running CodeQL.

## Example

```yaml
name: CodeQL
description: Run CodeQL Analysis on the repository

on:
  push:
    branches: [main]  # Or the default branch for the repository
  pull_request:
    types: [opened, synchronize, reopened]
  schedule:
    # https://docs.github.com/en/actions/using-workflows/events-that-trigger-workflows#schedule
    - cron: '* * * * *'

jobs:
  codeql:
    runs-on: ubuntu-latest
    permissions:
      actions: read
      contents: read
      security-events: write
    strategy:
      fail-fast: false
      matrix:
        language: [python, javascript]
        # CodeQL supports [ 'cpp', 'csharp', 'go', 'java', 'javascript', 'python', 'ruby' ]
        # Learn more about CodeQL language support at https://git.io/codeql-language-support
    steps:
      - name: Run CodeQL action with language ${{ matrix.language }}
        uses: open-reading-frame/codeql-action@main
        with:
          language: ${{ matrix.language }}
```
