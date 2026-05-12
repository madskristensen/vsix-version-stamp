# vsix-version-stamp

A GitHub Action to stamp the version number into a Visual Studio extension's `.vsixmanifest` and `source.extension.cs` files.

Works on **Linux and Windows** runners. Uses Python 3 — no extra tools required. Drop-in replacement for [timheuer/vsix-version-stamp](https://github.com/timheuer/vsix-version-stamp).

## Usage

```yaml
- name: Increment VSIX version
  id: vsix_version
  uses: madskristensen/vsix-version-stamp@v1
  with:
    manifest-file: src\MyExtension\source.extension.vsixmanifest
    vsix-token-source-file: src\MyExtension\source.extension.cs
```

## Inputs

| Input | Required | Description |
|-------|----------|-------------|
| `manifest-file` | ✅ | Path to `source.extension.vsixmanifest` |
| `vsix-token-source-file` | | Path to `source.extension.cs` for version token replacement |
| `version-number` | | Explicit version to set (e.g. `2.1.0`). If omitted, the GitHub run number is stamped into the third segment |
| `build-number` | | Build number for the third segment. Defaults to `github.run_number` |

## Outputs

| Output | Description |
|--------|-------------|
| `version-number` | The resulting version string (e.g. `1.0.123`) |

## Example workflow

```yaml
jobs:
  build:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4

      - name: Increment VSIX version
        id: vsix_version
        uses: madskristensen/vsix-version-stamp@v1
        with:
          manifest-file: src/MyExtension/source.extension.vsixmanifest
          vsix-token-source-file: src/MyExtension/source.extension.cs

      - name: Build
        run: msbuild MyExtension.sln /p:Configuration=Release

      - name: Upload artifact
        uses: actions/upload-artifact@v4
        with:
          name: MyExtension.vsix
          path: src/MyExtension/bin/Release/net48/MyExtension.vsix
```