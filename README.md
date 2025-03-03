


| <img src="./logo.png" width="340"/> | <h1 align="left">Fabric</h1> <p align="left">Fabric is a free, open network that connects people and services without central control. Built on [hyperdht](https://github.com/holepunchto/hyperdht), it works with [Nostr](https://github.com/nostr-protocol/nostr) and [Spaces](https://spacesprotocol.org) to let you find someone by their sovereign handle (like `@example`) or by a public key (an `npub`).</p><br /> |
|-------------------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|


**Fabric makes it easy to share who you are or find others in a decentralized world.** It’s like an address book for the internet’s sovereign future—built on [hyperdht](https://github.com/holepunchto/hyperdht), it links [Nostr](https://github.com/nostr-protocol/nostr) messages and [Spaces](https://spacesprotocol.org) (sovereign handles, like `@example`) so anyone can look up a name or an npub without needing a middleman.


**Note:** Fabric is not a replacement for Nostr relays! It's intended for publishing small pieces of discovery information. For example, you could share a [NIP-65](https://github.com/nostr-protocol/nips/blob/master/65.md) relay list for your npub! Fabric accepts replaceable and addressable Nostr events.


Fabric enables [spaces](https://spacesprotocol.org) to publish Nostr events and DNS records on the DHT without needing to store any bloat on the Bitcoin blockchain!

## Installation


```shell
npm install -g @spacesprotocol/fabric
```

## Querying Fabric with beam

`beam` is a query tool and publisher.

1. Querying Nostr events:

```
beam <pubkey_or_space> <event kind> <optional-d-tag>
```

2. Querying DNS records
```
beam @buffrr TXT
```

By default, beam grabs a list of trust anchors from your local spaces client at http://127.0.0.1:7225/root-anchors.json to verify spaces. This works if you’re running a [spaces client](https://github.com/spacesprotocol/spaces) with Bitcoin Core locally. But if you just want to try out Fabric, you can use an external service instead:

```shell
export FABRIC_REMOTE_ANCHORS=https://bitpki.com/root-anchors.json
```

Or add it when you run beam:

```shell
beam @buffrr TXT --remote-anchors https://bitpki.com/root-anchors.json
```

Note: This means you’re trusting BitPKI (or whoever you pick) to provide accurate anchors file, so its best to run your own Bitcoin full node + [spaces client](https://github.com/spacesprotocol/spaces) locally to generate your own, and keep it up to date.

## Publishing Records for a Space

1. **Create a DNS zone file** (e.g., `example.zone`):

```
@example. 3600 CLASS2 A   127.0.0.1
@example. 3600 CLASS2 TXT "hello world"
```

2. **Sign the zone file** with `space-cli`:

```
space-cli signzone @example example.zone > event.json
```

3. **Publish** with beam:

```
beam publish event.json
```

The network retains records for up to 48 hours. To refresh, run:

```
space-cli refreshanchor event.json
beam publish event.json
```

You can also distribute event.json to a Fabric service operator for continuous publication.

## Running a Fabric Node

To contribute to the network, run a Fabric node by specifying a reachable IP and port:

    fabric --host <public-ip-address> --port <public-port>

After about 30 minutes of uptime, your node becomes persistent.

## Contributing Bootstrap Nodes

We welcome more bootstrap nodes. To contribute:

1. Run a node with a reachable IP/port using the `--bootstrap` flag:

       fabric --host <public-ip-address> --port <public-port> --bootstrap

2. Submit a pull request updating `constants.js` with your node’s details.


## How does Fabric verify spaces?

1. The [Spaces client](https://github.com/spacesprotocol/spaces) performs Client Side Validation (CSV) reading Bitcoin blocks and verifying the protocol state, summarizing it all into a Merkle tree root
2. The client generates a trust anchors file that's used by Fabric to verify PUT requests.


## Encrypted Noise Connections

Basic support for encrypted connections over named spaces is available. Use `beam serve` and `beam connect`—follow the CLI instructions.

## Contributing

Contributions are welcome! Please submit issues, feature requests, or pull requests to help improve Fabric.

## License

This project is licensed under the Apache 2.0 License. See the LICENSE file for details.