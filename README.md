# Monolythium Networks Registry

Canonical source of truth for all Monolythium network configurations.

## Directory Structure

```
networks/
├── schema/
│   └── network.schema.json       # JSON Schema v7 for network configs
├── networks/
│   ├── localnet.json             # Local development network
│   ├── sprintnet.json            # Active testnet (EVM chain ID: 262146)
│   ├── testnet.json              # Public testnet (EVM chain ID: 6940)
│   └── mainnet.json              # Production mainnet (EVM chain ID: 6941)
├── index.json                    # Registry index with EVM chain ID map
└── .github/workflows/
    └── validate.yml              # CI validation workflow
```

## Network Configurations

### Localnet
- **Cosmos Chain ID**: `mono-local-1`
- **EVM Chain ID**: `262145` (`0x40001`)
- **Status**: Active
- **Purpose**: Local development and testing

### Sprintnet
- **Cosmos Chain ID**: `mono-sprint-1`
- **EVM Chain ID**: `262146` (`0x40002`)
- **Status**: Active
- **Purpose**: Public testnet for protocol validation

### Testnet
- **Cosmos Chain ID**: `mono_6940-1`
- **EVM Chain ID**: `6940` (`0x1B1C`)
- **Status**: Testing
- **Purpose**: Public testnet

### Mainnet
- **Cosmos Chain ID**: `mono_6941-1`
- **EVM Chain ID**: `6941` (`0x1B1D`)
- **Status**: Testing
- **Purpose**: Production mainnet

## EVM Chain ID Allocation

| Network | Chain ID | Hex | Status |
|---------|----------|-----|--------|
| Localnet | 262145 | 0x40001 | Active |
| Sprintnet | 262146 | 0x40002 | Active |
| Testnet | 6940 | 0x1B1C | Testing |
| Mainnet | 6941 | 0x1B1D | Testing |

**CRITICAL INVARIANT**: Sprintnet must always use EVM chain ID `262146`. This is enforced by CI validation.

## Validation

The CI workflow validates:

1. **JSON Schema Compliance**: All network configs must conform to `schema/network.schema.json`
2. **EVM Chain ID Uniqueness**: No duplicate EVM chain IDs allowed
3. **Sprintnet Invariant**: Sprintnet EVM chain ID must be exactly `262146`
4. **Hex/Decimal Consistency**: Hex representations must match decimal values
5. **Index Consistency**: `index.json` must match individual network configs

## Usage

### Add a New Network

1. Create `networks/<network-name>.json` following the schema
2. Update `index.json` to include the new network
3. Ensure unique EVM chain ID
4. Run validation locally before committing

### Update a Network Configuration

1. Edit the appropriate `networks/<network-name>.json` file
2. Update the `updated_at` timestamp
3. If EVM chain ID, cosmos chain ID, or status changes, update `index.json`
4. Ensure validation passes

### Local Validation

```bash
# Install ajv-cli
npm install -g ajv-cli

# Validate a specific network
ajv validate -s schema/network.schema.json -d networks/sprintnet.json

# Validate all networks
for config in networks/*.json; do
  ajv validate -s schema/network.schema.json -d "$config"
done
```

## Schema Fields

### Required Fields

- `network_name`: Human-readable network name
- `cosmos_chain_id`: Cosmos SDK chain identifier
- `evm_chain_id`: EIP-155 chain ID (integer)
- `genesis_sha256`: SHA-256 hash of genesis.json
- `seeds`: Array of seed nodes (node_id@host:port)
- `bootstrap_peers`: Array of persistent peers
- `network_status`: One of `active`, `deprecated`, `testing`
- `config_version`: Version of this config schema

### Optional Fields

- `evm_chain_id_hex`: Hex representation of EVM chain ID
- `genesis_url`: URL to genesis.json file
- `rpc_endpoints`: Object with RPC endpoint arrays
- `port_scheme`: Object with default port configuration
- `updated_at`: ISO 8601 timestamp

## Integration

Applications and services should:

1. Fetch `index.json` for network discovery
2. Load specific network configs as needed
3. Verify genesis file SHA-256 hash before use
4. Use canonical seed/peer lists for P2P networking
5. Respect the `network_status` field

## License

MIT
