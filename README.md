# Three Dog APK Intelligence API

Wallet-paid static intelligence for Android APK files. Upload an APK and receive machine-readable JSON without creating a service account.

## What it extracts

- Package, version, min SDK and target SDK
- Permissions, activities, services, receivers and providers
- Exported component signals
- Flutter, React Native, Cordova and Xamarin markers
- OkHttp, Retrofit, Ktor, Volley and Apollo markers
- Embedded URLs and domains
- Native ABIs and libraries
- Certificate-file hashes
- APK SHA-256 and archive metadata

## Live API

- Base URL: https://e72cc5cd51fdfc.lhr.life
- Health: https://e72cc5cd51fdfc.lhr.life/health
- OpenAPI: https://e72cc5cd51fdfc.lhr.life/openapi.json
- Discovery: https://e72cc5cd51fdfc.lhr.life/.well-known/apk-intel.json
- Analysis endpoint: `POST /v1/apk/quick`

## Price

**0.05 USDC per APK** on Solana mainnet.

Payment address:

`2LMjHRhKVbB1oWrwfd9g2zqVupreJwtpn4W38rBGbgCD`

USDC mint:

`EPjFWdd5AufqSSqeM2qN1xzybapC8G4wEGGkZwyTDt1v`

A request without payment returns HTTP `402` plus the machine-readable payment challenge.

## Call the API

1. Transfer at least 0.05 USDC to the payment address.
2. Wait for the Solana transaction to finalize.
3. Send the finalized transaction signature in `X-SOLANA-TRANSACTION`.

```bash
curl -H "X-SOLANA-TRANSACTION: SIGNATURE" \\
  -H "Content-Type: application/vnd.android.package-archive" \\
  --data-binary "@app.apk" \\
  https://e72cc5cd51fdfc.lhr.life/v1/apk/quick
```

The receipt verifier checks transaction success, recipient USDC balance increase, minimum amount, finalization, freshness and replay protection.

## Example response

```json
{
  "artifact": {
    "sha256": "...",
    "size": 12345678
  },
  "manifest": {
    "package": "com.example.app",
    "minSdk": "24",
    "targetSdk": "35"
  },
  "signals": {
    "httpStacks": ["okhttp", "retrofit"],
    "frameworks": ["flutter"]
  },
  "urls": ["https://api.example.com/v1"],
  "native": {
    "abis": ["arm64-v8a"]
  }
}
```

## Data handling

Uploaded APKs are processed in a temporary request directory. The analyzer does not contact endpoints discovered inside the APK.

## Service status

The current public hostname is an active reverse-tunnel endpoint. Open an issue if the endpoint is unavailable or if you need batch analysis.
