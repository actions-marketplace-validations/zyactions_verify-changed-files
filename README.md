# Verify Changed Files

![License: MIT][shield-license-mit]
[![CI][shield-ci]][workflow-ci]
[![Ubuntu][shield-platform-ubuntu]][job-runs-on]
[![macOS][shield-platform-macos]][job-runs-on]
[![Windows][shield-platform-windows]][job-runs-on]

A GitHub Action to verify file changes that occur during workflow execution.

## Features

- Lists all files that changed during a workflow execution
- Supports [glob patterns][glob-cheat-sheet] to restrict change detection to a subset of files
- Fast execution
- Scales to large repositories
- Supports all platforms (Linux, macOS, Windows)
- Does not use external dependencies

## Usage

### Detect changes to all files

```yaml
steps:
  - name: Checkout
    uses: actions/checkout@v3.1.0

  - name: Change Files
    run: |
      echo "test" > test/new.txt
      echo "test" > unstaged.txt

  - name: Verify Changed Files
    id: verify-changes
    uses: zyactions/verify-changed-files@master

  - name: Run only if one of the files has changed
    if: steps.verify-changes.outputs.changed-files != ''
    run: |
      echo "Changed Files:"
      echo "${{ steps.verify-changes.outputs.changed-files }}"

  - name: Run only if 'unstaged.txt' has changed
    if: contains(steps.verify-changes.outputs.changed-files, 'unstaged.txt')
    run: |
      echo "Detected changes to 'unstaged.txt'"
```

### Detect changes to specific files

```yaml
steps:
  - name: Checkout
    uses: actions/checkout@v3.1.0
    # ...
  - name: Verify Changed Files
    id: verify-changes
    uses: zyactions/verify-changed-files@master
    with:
      path: |
        test/*
        unstaged.txt
```

Check out the [glob pattern cheat sheet][glob-cheat-sheet] for reference. Multi line patterns must be specified without quotes.

## Inputs

```yaml
path:
  # A file, directory or wildcard pattern that describes which files to verify.
  # Supports multi-line strings. Matches all files, if set to the empty string.
  type: string
  default: '**/*'

separator:
  # A character- or string to be used to separate the individual file names in
  # the output list. Any filename containing this separator will be enclosed
  # in double quotes. Defaults to the newline character '\n', if set to the
  # empty string.
  type: string
  default: "\n"

exclude-ignored:
  # Whether to exclude '.gitignore' ignored files for added files detection.
  type: boolean
  default: true
```

## Outputs

```yaml
changed-files:
  # The list of all changed files, or an empty string.
  type: string
```

## Dependencies

This action does not use external dependencies.

> **Note**
>
> Using external GitHub Actions dependencies within an action negates the security benefits of pinning to a specific commit hash if they (the dependencies) are not also pinned.

## Versioning

Versions follow the [semantic versioning scheme][semver].

## License

Verify Changed Files Action is licensed under the MIT license.

[shield-license-mit]: https://img.shields.io/badge/License-MIT-blue.svg
[shield-ci]: https://github.com/zyactions/verify-changed-files/actions/workflows/ci.yml/badge.svg
[shield-platform-ubuntu]: https://img.shields.io/badge/Ubuntu-E95420?logo=ubuntu\&logoColor=white
[shield-platform-macos]: https://img.shields.io/badge/macOS-53C633?logo=apple\&logoColor=white
[shield-platform-windows]: https://img.shields.io/badge/Windows-0078D6?logo=windows\&logoColor=white
[workflow-ci]: https://github.com/zyactions/verify-changed-files/actions/workflows/ci.yml
[job-runs-on]: https://docs.github.com/en/actions/reference/workflow-syntax-for-github-actions#jobsjob_idruns-on
[glob-cheat-sheet]: https://docs.github.com/en/actions/using-workflows/workflow-syntax-for-github-actions#filter-pattern-cheat-sheet
[semver]:https://semver.org/
