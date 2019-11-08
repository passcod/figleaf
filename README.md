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
