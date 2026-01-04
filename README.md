# Monolythium Networks

Network configurations and genesis files for Monolythium blockchain networks.

## Networks

| Network | Status | Chain ID | Genesis |
|---------|--------|----------|---------|
| Sprintnet | Active | mono_262145-1 | [genesis.json](./sprintnet/genesis.json) |

## Usage

Download genesis for your network:

```bash
curl -o genesis.json https://raw.githubusercontent.com/monolythium/networks/main/sprintnet/genesis.json
```

Or use monoctl:

```bash
monoctl join --network Sprintnet --home ~/.monod
```
