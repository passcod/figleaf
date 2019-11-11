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

## Table of contents

Stricken items are not (or only partially) implemented yet.

- ~~[Definition](#definition-of-configuration-structure)~~
  + ~~[Arbitrary](#arbitrary-value)~~
  + ~~[Serde deserialisable](#serde-deserialisable-only)~~
    * ~~[Refresher](#refresher-on-serde-syntax)~~
    * ~~[Helpers](#serde-helpers)~~
  + ~~[Macro](#figleaf-macro)~~
    * ~~[Standalone](#standalone-use)~~
    * ~~[Alongside serde](#alongside-serde)~~
    * ~~[Derived fields](#derived-fields)~~
    * ~~[Switched fields]~~
    * ~~[Documentation comments]~~
    * ~~[Contextual documentation]~~
    * ~~[Conditionals]~~
- ~~[Generators]~~
  + ~~[Readme]~~
  + ~~[Example config file]~~
  + ~~[Man page]~~
  + ~~[JSON]~~
- ~~[Loading]~~
  + ~~[Async]~~
  + ~~[Blocking] /* async but with a quick block_on() wrapper */~~
  + ~~[Sync] /* without async at all (feature-gated) */~~
  + ~~[Singleton]~~
    * ~~[Once]~~
    * ~~[Lazy]~~
    * ~~[With main]~~
  + ~~[Recursive reconfiguration]~~
- ~~[In libraries]~~
  + ~~[Usage]~~
  + ~~[Discovery]~~
  + ~~[Overrides]~~
  + ~~[Excludes]~~
  + ~~[Remapping]~~
- ~~[Languages]~~
  + ~~[Defaults]~~
  + ~~[Compile-time selection]~~
  + ~~[Additional]~~
    * ~~[Via cargo features]~~
    * ~~[Via plugins]~~
  + ~~[Runtime selection]~~
  + ~~[File extentions and mimetypes]~~
  + ~~[Auto-detection]~~
  + ~~[Preprocessing]~~
- ~~[Environment]~~
  + ~~[Key format (prefix, etc)]~~
  + ~~[Value parsing]~~
  + ~~[Blobs]~~
- ~~[Arguments]~~
  + ~~[Wildcards / globs]~~
  + ~~[Positional]~~
  + ~~[Pico]~~
  + ~~[Clap]~~
    * ~~[From Figleaf]~~
    * ~~[From existing]~~
- ~~[Files]~~
  + ~~[Lookup paths]~~
  + ~~[Hierarchy]~~
  + ~~[Permissions and file attributes]~~
  + ~~[Overriding]~~
  + ~~[Includes]~~
    * ~~[Via `.d` folders]~~
    * ~~[Via language]~~
    * ~~[Via recursion]~~
    * ~~[Via preprocessor]~~
  + ~~[With a custom reader]~~
  * ~~[Memory-mapped]~~
- ~~[Network]~~
  + ~~[HTTP]~~
    * ~~[HTTPS]~~
    * ~~[HTTP/3]~~
  + ~~[TCP]~~
    * ~~[TLS]~~
    * ~~[Domain sockets]~~
  + ~~[UDP]~~
    * ~~[Passive]~~
    * ~~[Active]~~
    * ~~[QUIC]~~
    * ~~[Datagram sockets]~~
  + ~~[DNS]~~
    * ~~[mDNS]~~
- ~~[Databases]~~
  + ~~[Connection]~~
    * ~~[Configuration]~~
    * ~~[Reuse]~~
    * ~~[Pooling]~~
  + ~~[Relational]~~
    * ~~[SQLite]~~
    * ~~[MySQL]~~
    * ~~[Postgres]~~
    * ~~[MSSQL]~~
    * ~~[Cassandra]~~
    * ~~[Redshift]~~
  + ~~[Key-value]~~
    * ~~[Redis-like]~~
    * ~~[LevelDB-like]~~
    * ~~[Sled]~~
    * ~~[Etcd]~~
    * ~~[Consul]~~
    * ~~[Riak]~~
  + ~~[Document]~~
    * ~~[Mongo]~~
    * ~~[Couch]~~
- ~~[Platorm-specific]~~
  + ~~[D-Bus]~~
  + ~~[Windows COM]~~
  + ~~[Apple Events]~~
  + ~~[Virtual filesystems]~~
- ~~[Special]~~
  + ~~[Standard input]~~
  + ~~[Appended to binary]~~
  + ~~[Shared memory]~~
  + ~~[Keyring]~~
  + ~~[Clipboard]~~
  + ~~[EFI variables]~~
  + ~~[Hardware tokens]~~
  + ~~[Serial]~~
  + ~~[Block device]~~
  + ~~[Signals]~~
- ~~[Reloading]~~
  + ~~[Watching and polling]~~
  + ~~[Signals]~~
  + ~~[Reconfiguration]~~
  + ~~[Environment]~~
  + ~~[Files]~~
  + ~~[Network]~~
  + ~~[Database]~~
- ~~[Encryption]~~
  + ~~[Symmetric]~~
  + ~~[Public key]~~
    * ~~[Signing]~~
  + ~~[Layered]~~
  + ~~[Partial]~~
    * ~~[By method]~~
    * ~~[By file]~~
    * ~~[By key]~~
  + ~~[Secrets in memory]~~

## Definition of configuration structure

A lot of the magic of Figleaf comes from defining and describing the
datastructure that is the result of loading configuration. Generally, that
datastructure is a **field struct**, though it can also be an **enum** or
**tuple struct** or another kind of type.

There are three main categories of structure:

### Arbitrary value

While not recommended, as that makes a lot of Figleaf unavailable, it is
entirely possible to return a value that can be traversed dynamically instead
of parsing into a custom structure. This is made possible by the
[`arbitrary::Value`] type.

TODO: example

### Serde deserialisable only

While most of the power of Figleaf comes with the macro introduced in the next
section, a structure that simply derives Serde's [`Deserialize`] trait is
already and immediately useful, as Figleaf will parse configuration inputs into
that structure, and Serde's attributes can be used to add a lot of flexibility
to the structure itself.

#### Refresher on Serde syntax

(Always refer to [the Serde documentation] for details and exhaustive options.)

TODO: the derive, rename and rename all, the enum styles, skip, with_.

#### Serde helpers

Figleaf also provides its own helpers that augment the Serde deserialisation,
especially for the `with_*` attributes.

TODO: list helpers

The `arbitrary::Value` mentioned previously can also be used as a field
anywhere in a Serde structure, for partial fully-dynamic configuration values.

TODO: example

### `#[figleaf]` macro

This macro, applied on a configuration datastructure alone or alongside Serde,
enables the full power of Figleaf, through attributes on the structure, its
substructures, and fields.

The full list of options can be found [in the API documentation]. What follows
is an overview and reference of the basics and the most useful features.

#### Standalone use

It's possible to apply only the macro to a structure, without Serde. In this
case, the `parse=` top-level attribute is required. Its function takes an
[`arbitrary::Value`] and must return an [`error::Result<T>`] where `T` is the
structure the attribute is applied to.

```rust
use figleaf::prelude::*;

#[figleaf(parse = Config::parse)]
struct Config {
    key: String,
}

impl Config {
    fn parse(raw: &arbitrary::Value) -> figleaf::error::Result<Self> {
        let key = raw.get("key")?;
        Ok(Self { key })
    }
}
```

#### Alongside Serde

When the structure implements `Deserialize`, the `parse=` top-level attribute
isn't allowed (as that is implemented by Serde).

The macro implements various facilities described in this and later sections.
It is immediately useful: further customisation via attributes is described
below but the core benefits are already available with default settings as soon
as the macro is added.

```rust
use figleaf::prelude::*;

#[figleaf]
#[derive(Debug, Deserialize, Clone)]
struct Config {
    key: String,
}

// Load directly from struct:
let config = Config::load().await?;

// All figleaf::Loadable methods are available:
config.reload().await?;

// Generators are enabled:
dbg!(Config::generate_markdown());
```

TODO: Serde-specific integrations

#### Derived fields

Marking a field with `#[figleaf(derive_with = function)]` creates a
once-computed field: the value is computed once when the configuration is
loaded by calling the function provided with a reference to the
partially-constructed structure.

If the field implements `Default`, that is used for the initial value while
constructing the structure. Otherwise, the structure is partially constructed
using `MaybeUninit`. In any case, accessing the field being derived on the
passed-in partially-constructed structure is strongly discouraged and may be
undefined behaviour.

```rust
#[figleaf]
#[derive(Deserialize)]
struct Config {
    loaded: String,

    #[figleaf(derived_with = derivation)]
    derived: String,
}

fn derivation(partial: &Config) -> String {
  partial.loaded.trim().into()
}
```

#### Switched fields


