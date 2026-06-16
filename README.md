### Install Zoo CLI

Use the Zoo CLI in your Github workflows.

Example usage:
```yml
name: Install Zoo cli and convert demo
on:
  pull_request:
jobs:
  convert-with-powershell:
    runs-on: windows-latest
    steps:
      - uses: actions/checkout@v3
      - uses: KittyCAD/action-install-cli@v0.2.21
      - name: convert
        run: zoo file convert --output-format=stl test-file.obj ./
        shell: powershell
        env: 
          ZOO_TOKEN: ${{ secrets.ZOO_TOKEN }}
```

Make sure you [generate your `ZOO_TOKEN`](https://zoo.dev/account) and add it to your repo secrets

Be sure to look at our [other Github Actions](https://github.com/marketplace?type=actions&query=kittycad+).

## Privacy

This Action contacts Chainguard's licensing server to verify authorization. Connection metadata (IP address, GitHub repository identifier, timestamp, and any metadata encoded in the auth token) is transmitted to Chainguard, Inc. even if authorization is denied in accordance with our [Privacy Notice](https://www.chainguard.dev/legal/privacy-notice)
