# 1Cat Tunnel Client

This repository is the binary-only distribution repo for the 1Cat Tunnel client.

It intentionally does not contain source code.

## Install

```bash
npm install -g 1cat-tunnel-client
1cat-tunnel-client
```

## What is published here

- `tunnel-client-linux-amd64`
- `tunnel-client-windows-amd64.exe`
- `checksums.txt`

These files are attached to GitHub Releases and are used by the npm installer during `postinstall`.

## Setup flow

1. Deploy your server first.
2. Issue a node-specific bootstrap token from the server console.
3. Run `1cat-tunnel-client` on the customer machine.
4. Paste the bootstrap token and follow the setup wizard.
