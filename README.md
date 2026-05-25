# tokenpot.eth

Decentralized archive of **Tokenpot Capital** — the cryptocurrency investment fund that operated 2015–2019 and was the predecessor to [10102](https://10102.io).

Built by [10102](https://10102.io) · permanence and self-custody on Ethereum.

Live at: [tokenpot.eth.limo](https://tokenpot.eth.limo)

## Stack

Pure HTML / CSS / JS. No build step. Designed to outlast frameworks and survive on IPFS as a permanent archive.

```
public/
└── index.html      ← single file, inline CSS, embedded base64 images
```

`.github/workflows/omnipin.yml` deploys `./public` to IPFS via [Omnipin](https://github.com/omnipin/omnipin) and updates the ENS contenthash on `tokenpot.eth`.

## Deploy

See [DEPLOYMENT.md](./DEPLOYMENT.md). TL;DR: push to `main` after configuring the required secrets and variables. The workflow pins to Pinata + Lighthouse + 4everland and updates the ENS record.

## Local preview

```bash
cd public && python3 -m http.server 8000
# open http://localhost:8000
```
