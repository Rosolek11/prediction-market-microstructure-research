# Run the portfolio build

A source-free Windows x64 demonstration package is available under
[GitHub Releases](https://github.com/Rosolek11/prediction-market-microstructure-research/releases).

## What is included

- a precompiled, checksummed Rust executable;
- a one-click paper-mode launcher;
- a conservative example configuration;
- a local live dashboard;
- compressed JSONL data recording.

The package uses public market-data streams. Paper mode requires no wallet,
account, API key, private key, or login.

## Quick start

1. Download `prediction-market-recorder-windows-x64.zip` from Releases.
2. Verify the archive or executable against the published SHA-256 checksums.
3. Extract the entire archive.
4. Run `start-paper.bat`.
5. Open `http://127.0.0.1:8787` if the browser does not open automatically.
6. Stop the recorder with `Ctrl+C`.

The dashboard listens only on the local loopback interface. The package does not
contain or disclose any production IP address, remote dashboard URL, wallet
credential, deployment configuration, or captured production dataset.

## Runtime files

The application creates these local files:

- `recorder-settings.json`;
- `chainlink-atr-5s-cache.json`;
- `btc-5m-research.jsonl.gz`.

Recorded events are appended, allowing a longer research session to be analysed
as one continuous dataset.

## Safety boundary

The supplied launcher starts recorder and paper mode only. It does not enable
live orders. Paper fills remain simulations and can differ materially from real
execution because of latency, queue position, disappearing liquidity, and
slippage.

This build is a portfolio demonstration, not investment advice or a claim of
guaranteed profitability.
