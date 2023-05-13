# setup-regctl

This action builds [a Regctl][1] utility from sources or downloads a prebuilt binary and adds it to the `PATH`.

It supports GitHub-provided Linux, macOS, and Windows runners. The self-hosted runners may not work since they may
not have all the required software installed.

## Using the prebuilt binary

Officially, there are available binaries for all systems and architectures used by the GitHub runners.
Once the prebuilt binary is downloaded, it will always be checked against its signature available at [the official utility location][2].
If the signature is not valid, the Regctl utility will not be installed for safety reasons.

| OS/ARCH | amd64 | arm64 |
|---------|:-----:|:-----:|
| macOS   |   ✓   |   ✓   |
| Linux   |   ✓   |   ✓   |
| Windows |   ✓   |   ✗   |

### Usage

For installing the latest released prebuilt Regctl utility add the following entry to your Github workflow YAML file.
```yaml
steps:
  - uses: vookimedlo/setup-regctl@v1
```

Available arguments:

| Argument           | Default value | Description                                                                                                                                     |
|--------------------|---------------|-------------------------------------------------------------------------------------------------------------------------------------------------|
| `regctl-version`   | `'latest'`    | The required version of the Regctl utility. Use the `'latest'` for the latest tagged release or [a release tag from the official repository.][3] |
| `regctl-prebuilt`  | `'true'`      | Must be `'true'`, otherwise the Regctl utility will be built from sources.                                                                      |

Example:
```yaml
steps:
  - uses: vookimedlo/setup-regctl@v1
    with:
      regctl-version: 'v0.4.8'
```

## Build the binary from sources

| OS/ARCH | amd64 | arm64 |
|---------|:-----:|:-----:|
| macOS   |   ✓   |   ✓   |
| Linux   |   ✓   |   ✓   |
| Windows |   ✓   |  ⚠️   |

⚠️ Theoretically, the Windows ARM64 binary could be built, but no GitHub runner using this architecture
right now.

### Usage

For building the latest released Regctl utility from sources add the following entry to your Github workflow YAML file.
```yaml
steps:
  - uses: vookimedlo/setup-regctl@v1
    with:
      regctl-prebuilt: 'false'
```

Available arguments:

| Argument          | Default value | Description                                                                                                                                  |
|-------------------|---------------|----------------------------------------------------------------------------------------------------------------------------------------------|
| `go-version`      | `'stable'`    | The Go compiler version to use.                                                                                                              |
| `regctl-version`  | `'latest'`    | The required version of the Regctl utility. Use the `'latest'` for the latest tagged release or [a release tag from the official repository.][3] |
| `regctl-prebuilt` | `'true'`      | Must be `'false'`, otherwise the prebuilt Regctl utility will be downloaded.                                                                  |

Example:
```yaml
steps:
  - uses: vookimedlo/setup-regctl@v1
    with:
      go-version: '1.20.4'
      regctl-version: 'v0.4.8'
      regctl-prebuilt: 'false'
```


## The Regctl utility executable

Either the prebuilt binary is installed, or the binary is built from sources, the utility uses the same name.
The name is `regctl`, and on the Windows platform the complementary binary `regctl.exe` is
deployed. Both binaries are identical.

```yaml
steps:
  - uses: vookimedlo/setup-regctl@v1
  - shell: bash
    run: |
      regctl version
```

## Dependencies

This action depends on the following actions.

| Action                      | Description                                                             |
|-----------------------------|-------------------------------------------------------------------------|
| `actions/checkout`          | Downloads sources from the official repository.                         |
| `actions/setup-go`          | Provides the Go compiler used for the Regctl compilation.               |
| `sigstore/cosign-installer` | Provides the Cosign utility for checking the prebuilt Regctl signature. |


------

[1]: https://github.com/regclient/regclient
[2]: https://github.com/regclient/regclient/releases/download/latest/metadata.tgz
[3]: https://github.com/regclient/regclient/tags
