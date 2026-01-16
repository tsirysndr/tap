# tap

Multi-platform binary builder for [indigo tap](https://github.com/bluesky-social/indigo/tree/main/cmd/tap) - an AT Protocol sync utility.

## About

This repository provides pre-built binaries of the [tap](https://github.com/bluesky-social/indigo/tree/main/cmd/tap) utility from the [bluesky-social/indigo](https://github.com/bluesky-social/indigo) project. Tap is an AT Protocol sync utility that simplifies working with the AT Protocol firehose by handling connection management, verification, backfill, and filtering.

## What is tap?

Tap is a utility that:

- Verifies repo structure, MST integrity, and identity signatures
- Provides automatic backfill: fetches full repo history from PDS when adding new repos
- Offers filtered output: by DID list, by collection, or full network mode
- Guarantees ordering: live events wait for historical backfill to complete
- Supports multiple delivery modes: WebSocket with acks, fire-and-forget, or webhook
- Uses SQLite or Postgres backend
- Designed for moderate scale (millions of repos, 30k+ events/sec)

## Installation

### Download Pre-built Binaries

Download the latest release for your platform from the [Releases](https://github.com/tsirysandratraina/tap/releases) page.

#### Linux (AMD64)
```bash
wget https://github.com/tsirysndr/tap/releases/download/tap-v0.1.3/tap-v0.1.3-linux-amd64.tar.gz
tar -xzf tap-v0.1.3-linux-amd64.tar.gz
chmod +x tap
sudo mv tap /usr/local/bin/
```

#### Linux (ARM64)
```bash
wget https://github.com/tsirysndr/tap/releases/download/tap-v0.1.3/tap-v0.1.3-linux-arm64.tar.gz
tar -xzf tap-v0.1.3-linux-arm64.tar.gz
chmod +x tap
sudo mv tap /usr/local/bin/
```

#### macOS (ARM64)
```bash
wget https://github.com/tsirysndr/tap/releases/download/tap-v0.1.3/tap-v0.1.3-darwin-arm64.tar.gz
tar -xzf tap-v0.1.3-darwin-arm64.tar.gz
chmod +x tap
sudo mv tap /usr/local/bin/
```

### Verify Downloads

Each release includes SHA256 checksums. To verify your download:

```bash
# Download the checksum file
wget https://github.com/tsirysndr/tap/releases/download/tap-v0.1.3/tap-v0.1.3-darwin-arm64.tar.gz.sha256

# Verify the archive
sha256sum -c tap-v0.1.3-darwin-arm64.tar.gz.sha256
```

## Quick Start

```bash
# Run tap
tap run --disable-acks=true
# By default, the service uses SQLite at `./tap.db` and binds to port `:2480`.

# In a separate terminal, connect to receive events:
websocat ws://localhost:2480/channel

# Add a repo to track
curl -X POST http://localhost:2480/repos/add \
  -H "Content-Type: application/json" \
  -d '{"dids": ["did:plc:ewvi7nxzyoun6zhxrhs64oiz"]}' # @atproto.com repo
```

## Available Platforms

- **Linux AMD64** (x86_64)
- **Linux ARM64** (aarch64)
- **macOS ARM64** (Apple Silicon)

## Building from Source

If you prefer to build from source or need a platform not listed above:

```bash
git clone https://github.com/bluesky-social/indigo.git
cd indigo
git checkout tap-v0.1.3  # or your desired version
go build -o tap ./cmd/tap
```

## Configuration

Tap can be configured via environment variables or CLI flags. Key options include:

- `TAP_DATABASE_URL`: database connection string (default: `sqlite://./tap.db`)
- `TAP_BIND`: HTTP server address (default: `:2480`)
- `TAP_RELAY_URL`: AT Protocol relay URL (default: `https://relay1.us-east.bsky.network`)
- `TAP_FULL_NETWORK`: track all repos on the network (default: `false`)
- `TAP_COLLECTION_FILTERS`: comma-separated collection filters (e.g., `app.bsky.feed.post`)
- `TAP_DISABLE_ACKS`: fire-and-forget mode (default: `false`)
- `TAP_WEBHOOK_URL`: webhook URL for event delivery

See the [official tap documentation](https://github.com/bluesky-social/indigo/tree/main/cmd/tap) for complete configuration options.

## Documentation

For comprehensive documentation, including:

- HTTP API endpoints
- Event format details
- Delivery modes
- Network boundary modes
- Collection filtering
- Backfill behavior
- Authentication setup

Please refer to the [official tap README](https://github.com/bluesky-social/indigo/blob/main/cmd/tap/README.md).

## Client Libraries

- **TypeScript**: [@atproto/tap](https://github.com/bluesky-social/atproto/tree/main/packages/tap)

## Release Process

This repository automatically builds and releases tap binaries when new tags are pushed. The build process:

1. Clones the indigo repository at the specified tag
2. Builds tap for Linux (AMD64 & ARM64) and macOS (ARM64)
3. Creates tar.gz archives with SHA256 checksums
4. Uploads artifacts to GitHub Releases

## Version Tracking

Binary releases follow the upstream indigo tap versioning. Check the [indigo releases](https://github.com/bluesky-social/indigo/releases) for changelogs and version history.

## Support

⚠️ **Note**: Tap is still in beta and may have bugs.

- For tap-specific issues: [indigo issues](https://github.com/bluesky-social/indigo/issues)
- For binary distribution issues: [this repository's issues](https://github.com/tsirysandratraina/tap/issues)

## License

This repository provides pre-built binaries of tap from the indigo project. The tap utility and indigo codebase are licensed under the MIT License. See the [indigo repository](https://github.com/bluesky-social/indigo) for license details.

## Related Projects

- [indigo](https://github.com/bluesky-social/indigo) - Go implementation of AT Protocol services
- [@atproto/tap](https://github.com/bluesky-social/atproto/tree/main/packages/tap) - TypeScript client for tap
- [AT Protocol](https://atproto.com/) - The AT Protocol specification
