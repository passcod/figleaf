# Figleaf

Experiment around [a modern-style `config`][config-111].

[config-111]: https://github.com/mehcode/config-rs/issues/111

 - **Not published on crates.io**
 - MSRV: latest stable, no version bump
 - License: [Artistic 2.0](./LICENSE)
 - API Documentation: https://passcod.name/figleaf/

## Quick-start

```toml
figleaf = { git = "https://github.com/passcod/figleaf", version = "0.1" }
```

```rust
use figleaf::prelude::*;

#[derive(Clone, Debug, Deserialize)]
struct Settings {
  username: String,
  password: String,
  friends: Vec<String>,

  #[serde(default)]
  lollipop: bool,

  lunch: Lunch,
}

#[derive(Clone, Debug, Deserialize)]
struct Lunch {
  #[serde(rename = "type")]
  kind: String,
  quantity: u8,
}

let settings: Settings = figleaf::auto().await?;
dbg!(settings);
```

This will read from:

 - Environment: `CRATENAME_USERNAME`, etc.
 - Config files named `cratename.ext`, where `ext` is any of the
   [supported formats][formats], in a hierarchy of overrides:
   + Local: working directory
   + Runtime: `/run` and equivalents
   + User: `XDG_CONFIG_PATH/CRATENAME/` and equivalents
   + System: `/etc/CRATENAME/` and equivalents
   + Vendor: `/usr/lib/CRATENAME/` and equivalents

Overrides are top-level only and additive for collections. That is:

```toml
# ./cratename.toml

username = "yourie"
password = "3194EA270314DF0A8D9B2BD"
friends = ["jaimie", "nickki"]
lunch = { type = "sandwich", quantity = 1 }


# /etc/cratename/cratename.toml

username = "brenden"
lunch = { type = "sushi", quantity = 4 }
friends = [
  "gostabo",
  "valrie",
  "joss"
]
```

will load as:

```rust
Settings {
    username: "yourie",
    password: "3194EA270314DF0A8D9B2BD",
    friends: [
        "gostabo",
        "valrie",
        "joss",
        "jaimie",
        "nickki",
    ],
    lollipop: false,
    lunch: Lunch {
        kind: "sandwich",
        quantity: 1,
    },
}
```

## Configuration

- [Definition]
  + [Arbitrary]
  + [Serde deserialisable]
    * [Refresher]
    * [Helpers]
  + [Macro]
    * [Standalone]
    * [Alongside serde]
    * [Derived fields]
    * [Switched fields]
    * [Documentation comments]
    * [Contextual documentation]
- [Generators]
  + [Readme]
  + [Example config file]
  + [Man page]
  + [JSON]
- [Loading]
  + [Async]
  + [Blocking] /* async but with a quick block_on() wrapper */
  + [Sync] /* without async at all (feature-gated) */
  + [Singleton]
    * [Once]
    * [Lazy]
    * [With main]
  + [Recursive reconfiguration]
- [In libraries]
  + [Usage]
  + [Discovery]
  + [Overrides]
  + [Excludes]
  + [Remapping]
- [Languages]
  + [Defaults]
  + [Compile-time selection]
  + [Additional]
    * [Via cargo features]
    * [Via plugins]
  + [Runtime selection]
  + [File extentions and mimetypes]
  + [Auto-detection]
  + [Preprocessing]
- [Environment]
  + [Key format (prefix, etc)]
  + [Value parsing]
  + [Blobs]
- [Arguments]
  + [Wildcards / globs]
  + [Positional]
  + [Pico]
  + [Clap]
    * [From Figleaf]
    * [From existing]
- [Files]
  + [Lookup paths]
  + [Hierarchy]
  + [Permissions and file attributes]
  + [Overriding]
  + [Includes]
    * [Via `.d` folders]
    * [Via language]
    * [Via recursion]
    * [Via preprocessor]
  + [With a custom reader]
  * [Memory-mapped]
- [Network]
  + [HTTP]
    * [HTTPS]
    * [HTTP/3]
  + [TCP]
    * [TLS]
    * [Domain sockets]
  + [UDP]
    * [Passive]
    * [Active]
    * [QUIC]
    * [Datagram sockets]
  + [DNS]
    * [mDNS]
- [Databases]
  + [Connection]
    * [Configuration]
    * [Reuse]
    * [Pooling]
  + [Relational]
    * [SQLite]
    * [MySQL]
    * [Postgres]
    * [MSSQL]
    * [Cassandra]
    * [Redshift]
  + [Key-value]
    * [Redis-like]
    * [LevelDB-like]
    * [Sled]
    * [Etcd]
    * [Consul]
    * [Riak]
  + [Document]
    * [Mongo]
    * [Couch]
- [Platorm-specific]
  + [D-Bus]
  + [Windows COM]
  + [Apple Events]
  + [Virtual filesystems]
- [Special]
  + [Standard input]
  + [Appended to binary]
  + [Shared memory]
  + [Keyring]
  + [Clipboard]
  + [EFI variables]
  + [Hardware tokens]
  + [Serial]
  + [Block device]
  + [Signals]
- [Reloading]
  + [Watching and polling]
  + [Signals]
  + [Reconfiguration]
  + [Environment]
  + [Files]
  + [Network]
  + [Database]
- [Encryption]
  + [Symmetric]
  + [Public key]
    * [Signing]
  + [Layered]
  + [Partial]
    * [By method]
    * [By file]
    * [By key]
  + [Secrets in memory]
