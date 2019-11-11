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
    * ~~[Switched fields](#switched-fields)~~
    * ~~[Documentation comments](#documentation-comments)~~
    * ~~[Contextual documentation](#contextual-documentation)~~
    * ~~[Conditionals](#runtime-field-conditions)~~
- ~~[Generators](#generators)~~
  + ~~[Readme](#readme)~~
  + ~~[Example config file](#example-configuration-file)~~
  + ~~[Man page](#man-page)~~
  + ~~[JSON](#json)~~
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

**In most cases, prefer derived fields.** However, in some cases, having the
loaded value hanging around is undesirable, or you may need to consume the
loaded value to create the derived one and don't want to add an `Option` in.

Whatever the reason, switched fields have one type when loading, and another
type once constructed. They can also be the same type in both cases, but with
the loaded value being passed as owned to the switching function.

```rust
#[figleaf]
#[derive(Deserialize)]
struct Config {
    #[figleaf(switch_from = usize, switch_with = switcher)]
    switched: String,
}

fn switcher(value: usize, _partial: &Config) -> String {
  format!("{}", value * 2)
}
```

While merely discouraged in the case of derived fields, **accessing a switched
field from the `partial` reference is strictly forbidden** and _is always
undefined behaviour_.

A switched field cannot also be a derived field and vice versa.

The `switch_with` function defaults to calling `.into()` if not provided.

#### Documentation comments

Documentation comments on fields and the structure itself are used to render
description and help text in generated representations:

- error messages
- readme generator
- man page generator
- example config file generator

To disable using this documentation, use the `#[figleaf(no_doc)]` attribute. To
disable this behaviour for all fields, add `#[figleaf(opt_in_docs)]` to the
top-level. When that is set, `#[figleaf(use_doc)]` on fields can be used to opt
in on a field-by-field basis.

#### Contextual documentation

Some documentation may be superfluous in some formats or only make sense in
others. It is possible to include or exclude all or part of any documentation
comment based on the generation target, in two different forms: inline to doc
comments, or as attributes.

```rust
/// The first documentation paragraph/line.
///
/// Common information.
///
/// # [figleaf(target = "man")]
///
/// This section is only shown on the man page.
///
/// # [figleaf(not(target = "rustdoc"))]
///
/// This section is not shown in the rustdoc output.
field: Item,
```

```rust
#[figleaf(target_doc("man", "This string replaces the documentation entirely for man pages"))]
#[figleaf(target_doc("commonmark", "This replaces the readme description")]
field: Item,
```

These doc targets are available by default:

- `rustdoc`: default/rustdoc output
- `man`: man pages
- `example`: example config file
- `commonmark`: readme/commonmark/markdown documentation

#### Runtime field conditions

Using conditional attributes, fields can be skipped or included based on
_runtime_ conditions. For compile-time conditionals, use Rust's own `cfg`
attributes.

```rust
#[figleaf]
#[derive(Deserialize)]
struct Config {
    #[figleaf(skip_on(source = "lang:toml"))]
    skipped_for_toml: bool,

    #[figleaf(only_on(source = "lang:dhall"))]
    only_when_loading_from_dhall: bool,

    #[figleaf(skip_on(custom = "skipper"))]
    custom_runtime_skip: bool,

    #[figleaf(only_on(custom = "includer"))]
    custom_runtime_include: bool,
}

fn skipper(_partial: &Config) -> bool {
    /* skip = */ true
}

fn includer(_partial: &Config) -> bool {
    /* include = */ true
}
```

Available conditional attributes:

- `lang:*`: configuration language used to load this structure
- `custom`: custom function
- `reload`: `"true"` when the config is being reloaded
- `singleton`: `"true"` when loading as a singleton
- `source`: `file`, `env`, `args`, `db:*`...
- TODO: more

## Generators

Figleaf comes several generators that create useful output from your
configuration definition. Due to technical limitations, these need to be called
from your own code and return strings — you can then output to screen or to
file as you see fit.

```rust
use figleaf::prelude::*;

#[figleaf]
#[derive(Deserialize)]
struct Config {
    field: String,
}

println!("{}", Config::generate(generators::Output::JSON));
```

### Readme

You can output a readme fragment describing your configuration file in various
readme formats:

- `plain`
- `commonmark` (aka Markdown)
- `asciidoc`

TODO: output example

### Example configuration file

You can output a configuration file with all fields commented out and with
comments containing the documentation for each field. Supported languages:

- `toml`
- `dhall`
- `yaml`

TODO: output example

Because JSON does not support comments, it is not supported.

### Man page

You can output a man page (roff). This will be named `CRATE_NAME(5)` (Section 5
is for file formats and configuration files.) You can opt to specify a
configuration language to use for the man page, or it will be written in a
language-independent style.

TODO: output example

### JSON

The JSON output format describes everything that Figleaf knows about your
configuration definition as well as its own configuration (version, which
features were enabled, etc). This format is regulated by semver so it can be
relied on.

See the [`generators::json`] module documentation for the exhaustive reference.

