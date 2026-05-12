# Security Policy

## Reporting a vulnerability

If you believe you have found a security vulnerability in Sentistic
Flow, **please do not open a public GitHub issue**. Instead, email a
detailed report to **security@sentistic.com**.

Please include:

- A clear description of the issue and its impact
- Steps to reproduce
- Affected version(s) (Help → About in the app)
- Operating system

We aim to acknowledge reports within **2 business days** and provide a
substantive response within **10 business days**.

## Supported versions

Only the most recent published release receives security updates.
Always upgrade to the latest version listed on the
[Releases page](../../releases/latest) before reporting an issue.

## Verifying downloads

Every release publishes a signed `version.json` document containing
SHA-256 hashes for each binary. Always verify the hash of any
download before installation:

```bash
# macOS / Linux
shasum -a 256 SentisticFlow-*.dmg

# Windows
Get-FileHash .\SentisticFlowSetup-*.exe -Algorithm SHA256
```

Compare the result against the `sha256` field in
[`version.json`](../../releases/latest/download/version.json).
