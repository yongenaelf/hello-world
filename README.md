# AElf DevContainer (BETA)

To use this devcontainer, click "Use this template" > "Create a new repository".

## Automatic deployment with GitHub Actions

To automatically deploy to AElf Testnet, copy the content below and save to `.github/workflows/main.yml`:

```yml
name: "main"
on: push

jobs:
  build:
    runs-on: ubuntu-latest
    steps:
      - name: Checkout (GitHub)
        uses: actions/checkout@v3

      - name: Run in dev container
        uses: devcontainers/ci@v0.3
        with:
          cacheFrom: ghcr.io/yongenaelf/aelfinity-workshop
          push: never
          runCmd: |
            cd src
            dotnet build
            cd ../test
            dotnet test

      - name: Deploy to testnet
        uses: yongenaelf/aelf-testnet-deploy-action@v1.4.2
        with:
          private-key: ${{ secrets.PRIVATEKEY }}
          wallet-address: ${{ secrets.WALLET_ADDRESS }}
```
