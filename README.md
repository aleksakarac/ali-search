# ali - AliExpress Product Search CLI

A terminal-based tool for searching, filtering, and comparing AliExpress products. Single-file Python script with zero manual dependency management (uses [uv](https://docs.astral.sh/uv/) inline script metadata).

Designed to be used by humans **and** by other Claude Code instances via `--json` output.

## Setup

1. Install [uv](https://docs.astral.sh/uv/) if you don't have it
2. Get a free API key (5,000 requests/month) at https://www.omkar.cloud/auth/sign-up?redirect=/api-key
3. Save your key:

```bash
# Option A: environment variable
export ALIEXPRESS_API_KEY="your-key"

# Option B: save to config file
ali save-key your-key
# Stored at ~/.config/ali/api_key (mode 600)
```

## Install

```bash
# Copy to your PATH
cp ali ~/bin/ali
chmod +x ~/bin/ali

# Or run directly
uv run ali search "ESP32 board"
```

## Usage

### Search products

```bash
ali search "ESP32 development board"
ali search "18650 battery holder" --pages 3 --sort price
ali search "relay module" --max-price 5 --min-rating 4.5 --min-orders 100
ali search "OLED display" --sort price --limit 10
```

### Get product details

```bash
ali details 1005007170995524
```

### Compare products

```bash
ali compare 1005007170995524 1005006789012345 1005008901234567
```

### Sort options

| Flag | Description |
|------|-------------|
| `--sort orders` | Most ordered first (default) |
| `--sort price` | Cheapest first |
| `--sort rating` | Highest rated first |
| `--sort discount` | Biggest discount first |

### Filter options

| Flag | Description |
|------|-------------|
| `--max-price 10` | Only products under $10 |
| `--min-rating 4.5` | Only products rated 4.5+ |
| `--min-orders 100` | Only products with 100+ orders |
| `--limit 20` | Show top 20 results |
| `--pages 3` | Fetch 3 pages of results |

## Machine-readable output

All commands support `--json` for structured output, making it easy for scripts or other AI agents to consume:

```bash
# JSON output for piping/parsing
ali search "ESP32" --json | jq '.[0]'
ali details 1005007170995524 --json
ali compare ID1 ID2 --json

# Example: get cheapest 5 products as JSON
ali search "arduino nano" --sort price --limit 5 --json
```

## API

Uses the [Omkar.cloud AliExpress Scraper API](https://github.com/omkarcloud/aliexpress-scraper). Free tier: 5,000 requests/month.

## License

MIT
