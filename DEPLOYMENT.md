# Deployment

This site auto-deploys to IPFS and updates the `tokenpot.eth` ENS contenthash on every push to `main`, via [Omnipin](https://github.com/omnipin/omnipin). Workflow at `.github/workflows/omnipin.yml`.

## One-time setup (if you haven't already done it for another ENS site)

You only need to do these once across your portfolio of ENS site repos. If you've already deployed `rogerfederer.eth` or any other ENS site via this template, skip to step 6.

### 1. Deploy wallet
Generate a fresh wallet whose only purpose is contenthash updates. Do NOT reuse your main wallet.

```bash
cast wallet new
```

Save the private key (66-char hex with `0x` prefix). Fund the address with ~$15 of ETH.

### 2. Approve the deploy wallet on the ENS Public Resolver 3
The deploy wallet needs permission to call `setContenthash` on names you own. One-time setup:

- Open `https://etherscan.io/address/0xF29100983E058B709F3D539b0c765937B804AC15#writeContract`
- Connect with the wallet that owns `tokenpot.eth` (and your other ENS names)
- Call `setApprovalForAll(operator, approved)`:
  - `operator` = deploy wallet's address
  - `approved` = `true`
- Sign the transaction

That deploy wallet can now update records on any name you own resolved by Resolver 3 — never transfer them.

### 3. Pinning provider accounts
- Pinata — pinata.cloud (use JWT, not API key/secret pair)
- Lighthouse — lighthouse.storage
- 4everland — dashboard.4everland.org → Storage → 4Ever Pin → Access token

## Configure this repo's secrets

The fast path is `~/DevMac/claude-config/scripts/setup-ens-site-secrets.sh`:

```bash
~/DevMac/claude-config/scripts/setup-ens-site-secrets.sh 10102-labs/tokenpot-eth tokenpot.eth
```

This sets all 4 secrets + the `OMNIPIN_ENS` variable from your local `~/DevMac/claude-config/secrets/ens-deploy.env` file.

## Or manually

In **Settings → Secrets and variables → Actions**:

**Variables**:
- `OMNIPIN_ENS` = `tokenpot.eth`
- `OMNIPIN_SAFE` = (optional)

**Secrets**:
- `OMNIPIN_PK`
- `OMNIPIN_PINATA_TOKEN`
- `OMNIPIN_LIGHTHOUSE_TOKEN`
- `OMNIPIN_4EVERLAND_TOKEN`

## Trigger deploy

Push to `main`, or use **Run workflow** on the Actions tab. The workflow:
1. Pins `./public/` to all three providers
2. Calls `setContenthash` on `tokenpot.eth`

## Verify

- `https://tokenpot.eth.limo`
- Direct IPFS via the CID in the workflow logs
- 5–15 min for ENS gateway cache to refresh

## Notes

- Job is gated on `OMNIPIN_ENS != ''` — no failure emails on unconfigured forks.
- Pure HTML, no build step → IPFS receives exactly what's in `public/`.
- All authentic 2015–2019 fund content + NAV gauge chart + recognition awards preserved from the original deployment.
