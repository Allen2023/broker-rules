# Broker Rules

Broker routing rules for popular cross-border brokerage and wealth platforms used by Chinese-speaking communities. The goal is to route these services through one fixed-exit policy such as `Broker`, making it easier to keep a stable outbound IP across Surge, Shadowrocket, Quantumult X, Clash, Stash, and Loon.

Current coverage:

- Futu / moomoo [富途牛牛]
- Longbridge [長橋證券]
- Tiger Brokers [老虎證券]
- Charles Schwab [嘉信理財]

Rule counts:

- `DOMAIN-SUFFIX`: 49
- `DOMAIN`: 10
- `IP-CIDR`: 18
- `TOTAL`: 77

## Structure

- [Broker.list](./Broker.list)
  Canonical source data using a rule-set syntax compatible with Surge, Shadowrocket, and Loon.
- [rule/Surge/Broker/Broker.list](./rule/Surge/Broker/Broker.list)
- [rule/Shadowrocket/Broker/Broker.list](./rule/Shadowrocket/Broker/Broker.list)
- [rule/Loon/Broker/Broker.list](./rule/Loon/Broker/Broker.list)
- [rule/QuantumultX/Broker/Broker.list](./rule/QuantumultX/Broker/Broker.list)
- [rule/Clash/Broker/Broker.yaml](./rule/Clash/Broker/Broker.yaml)
- [rule/Stash/Broker/Broker.yaml](./rule/Stash/Broker/Broker.yaml)

## Usage

This repository is published at `Allen2023/broker-rules`.

### Surge

```ini
[Proxy Group]
Broker = select, HK-Fixed-IP, US-Fixed-IP

[Rule]
RULE-SET,https://raw.githubusercontent.com/Allen2023/broker-rules/main/rule/Surge/Broker/Broker.list,Broker
```

### Shadowrocket

```ini
[Rule]
RULE-SET,https://raw.githubusercontent.com/Allen2023/broker-rules/main/rule/Shadowrocket/Broker/Broker.list,Broker
```

### Loon

```ini
[Rule]
RULE-SET,https://raw.githubusercontent.com/Allen2023/broker-rules/main/rule/Loon/Broker/Broker.list,Broker
```

### Quantumult X

```ini
[filter_remote]
https://raw.githubusercontent.com/Allen2023/broker-rules/main/rule/QuantumultX/Broker/Broker.list, tag=Broker, force-policy=Broker, enabled=true
```

Notes:

- The Quantumult X file already uses `Broker` as the policy name.
- If you want a different policy name, edit the third column in the file or rely on `force-policy`.

### Clash

```yaml
rule-providers:
  broker:
    type: http
    behavior: classical
    url: https://raw.githubusercontent.com/Allen2023/broker-rules/main/rule/Clash/Broker/Broker.yaml
    path: ./ruleset/broker.yaml
    interval: 86400

rules:
  - RULE-SET,broker,Broker
```

### Stash

```yaml
rule-providers:
  broker:
    type: http
    behavior: classical
    url: https://raw.githubusercontent.com/Allen2023/broker-rules/main/rule/Stash/Broker/Broker.yaml
    path: ./ruleset/broker.yaml
    interval: 86400

rules:
  - RULE-SET,broker,Broker
```

## Sources

This ruleset fully incorporates the relevant brokerage entries from the following upstream rule sources:

- `koo17173/rule-set`
  - full `futu.list`
  - full `longbridge` section from `Stocks.list`
  - all 3 additional Futu IP entries from the `futu` section in `Stocks.list`
- `blackmatrix7/ios_rule_script`
  - full `TigerFintech.list`

On top of those upstream datasets, this repository adds official domains used by quote delivery, Level 2 data, OpenAPI, login flows, and support centers, plus a small set of observable entry IPs captured on `2026-05-30`.

## Acknowledgements

Many thanks to the maintainers of the upstream rule projects, especially `koo17173/rule-set` and `blackmatrix7/ios_rule_script`. Their existing work provided the baseline coverage that made this focused broker ruleset possible. This repository builds on that foundation, verifies the broker-specific portions, and extends them for fixed-exit routing scenarios.

## Maintenance Notes

- Prefer domain-based matching as the primary routing method. `IP-CIDR` entries are best treated as supplemental coverage.
- Charles Schwab currently relies heavily on CDN edge delivery, so domain rules are more stable than hard-coded CDN IPs.
- Longbridge and Tiger Brokers both use dedicated quote, trade, and OpenAPI endpoints for market data and Level 2 flows, so website-only coverage is usually not enough.
- If you want to expand this repository later, consider splitting additional providers such as Huatai, East Money, or Tonghuashun into separate rule-sets to keep the scope clean.

## Upstream References

- [koo17173/rule-set](https://github.com/koo17173/rule-set/tree/main)
- [blackmatrix7/ios_rule_script](https://github.com/blackmatrix7/ios_rule_script)
- [Longbridge OpenAPI](https://open.longbridge.com/zh-CN/docs/quote/overview)
- [Tiger OpenAPI Docs](https://docs.itigerup.com/docs/push-quote)
- [moomoo OpenAPI Docs](https://openapi.moomoo.com/moomoo-api-doc/en/opend/opend-intro.html)
- [Charles Schwab Chinese-language Services](https://www.schwab.com/chinese-language-services-en)
