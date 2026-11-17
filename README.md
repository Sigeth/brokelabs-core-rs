# brokelabs-core-rs

Rust crates shared by the BrokeLabs [api](https://github.com/Sigeth/brokelabs-api) and [gateway](https://github.com/Sigeth/brokelabs-gateway).

| Crate | What |
|---|---|
| `brokelabs-types` | Rust types of the BrokeLabs custom resources (`Challenge`, `ChallengeInstance`, `CheckRun`…), **generated** with [kopium](https://github.com/kube-rs/kopium) from the CRDs of [brokelabs-core-go](https://github.com/Sigeth/brokelabs-core-go), which is the source of truth. |
| `brokelabs-auth` | Short-lived EdDSA access tokens: the api mints them (`Signer`) and publishes a JWKS document, the gateway verifies them (`Verifier`). |

## Usage

The crates are not on crates.io yet. Depend on them by tag:

```toml
[dependencies]
brokelabs-types = { git = "https://github.com/Sigeth/brokelabs-core-rs", tag = "v0.8.0" }
brokelabs-auth = { git = "https://github.com/Sigeth/brokelabs-core-rs", tag = "v0.8.0" }
# brokelabs-types needs exactly one Kubernetes version feature, chosen by the binary:
k8s-openapi = { version = "0.29", features = ["v1_34"] }
```

## Updating the generated types

The core-go tag is pinned in `CORE_GO_VERSION`.

```sh
cargo install kopium --version 0.24.1 --locked   # once
make generate          # from the pinned tag (what CI checks)
make generate-local    # from ../core-go, while developing a contract change
```

## Development

```sh
make lint test deny
```

`deny.toml` only allows MIT-compatible licenses. It also ignores two advisories, each with its reason. Both affect RSA/ECDSA code that `jsonwebtoken` compiles in but that BrokeLabs never uses: `Verifier` only accepts EdDSA, and a test checks that.

## License

[MIT](LICENSE)
