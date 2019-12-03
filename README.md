# Figleaf

_“Crackfic-taken-seriously in the form of an open-source project readme,” the configuration library._

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
  [default supported languages](#the-default-set), in a hierarchy
  of overrides:
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

- ~~[Concepts]~~
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
- ~~[Loading](#loading)~~
  + ~~[Async](#async)~~
  + ~~[Blocking](#blocking)~~
  + ~~[Sync](#all-synchronous)~~
  + ~~[Singleton](#singleton)~~
    * ~~[Eager](#static-config-loaded-eagerly)~~
    * ~~[Lazy](#static-config-loaded-lazily)~~
    * ~~[With main](#dynamic-config-in-main)~~
- ~~[Recursive reconfiguration](#recursive-reconfiguration)~~
- ~~[In libraries](#in-libraries)~~
  + ~~[Usage]~~
  + ~~[Discovery]~~
  + ~~[Overrides]~~
  + ~~[Excludes]~~
  + ~~[Remapping]~~
- ~~[Languages](#languages)~~
  + ~~[Defaults](#the-default-set)~~
  + ~~[Compile-time selection](#compile-time-selection)~~
  + ~~[Additional](#using-additional-languages)~~
  + ~~[Runtime selection](#runtime-selection)~~
  + ~~[File extentions and mimetypes](#file-exensions-and-mimetypes)~~
  + ~~[Auto-detection](#auto-detection)~~
- ~~[Sources](#sources)~~
  + ~~[Features](#features)~~
  + ~~[Filtering](#filtering)~~
  + ~~[Dynamic](#dynamic)~~
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
  + ~~[Windows Registry]~~
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
  + ~~[From hardware module]~~
  + ~~[Secrets in memory]~~
- ~~[Bindings]~~
  + ~~[C/C++]~~
  + ~~[Node.js]~~
  + ~~[Deno]~~ /* 1) via wasm, with deno-sided fs/env/args links; 2) as native op crate once that lands in deno */
  + ~~[WASM]~~
  + ~~[Ruby]~~
  + ~~[PHP]~~
  + ~~[Swift]~~
- ~~[Appendices]~~
  + ~~[Feature reference]~~
  + ~~[Optimisation guidelines]~~
  + ~~[Pre-built artifacts]~~

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

## Loading

Figleaf supports different APIs for loading the configuration. The main
distinction is around asynchronicity. There is also a singleton style with
several variants.

### Async

The default loading style uses async:

```rust
use figleaf::prelude::*;

#[figleaf]
#[derive(Deserialize)]
struct Config { etc: bool }

let config = Config::auto().await?;
```

The [`auto`] function selects all defaults and finalises the builder, which
resolves into a Future which is then `await`ed into a Result.

Constructing a builder manually:

```rust
use figleaf::prelude::*;

#[figleaf]
#[derive(Deserialize)]
struct Config { etc: bool }

let builder = Config::build()
  .basename("parka")
  .languages(&[Language::TOML, Language::JSON])
  .finalise()
  .await?;
```

### Blocking

If dealing with awaiting futures is too much trouble, there is a blocking API
which embeds an async executor. To reduce depedencies in the default case, this
is behind a feature.

```toml
[dependencies.figleaf]
version = "..."
features = ["load:blocking"]
```

```rust
use figleaf::prelude::*;

#[figleaf]
#[derive(Deserialize)]
struct Config { etc: bool }

let builder = Config::build()
  .basename("parka")
  .languages(&[Language::TOML, Language::JSON])
  .finalise_blocking()?;
```

Note the different finalise function.

### All-synchronous

Embedding an executor adds a lot of dependencies, and using async IO does too,
and may be complicated in some environments. Whatever the reason, there is also
an all-synchronous API, which uses synchronous IO from the stdlib instead of
async IO:

```toml
[dependencies.figleaf]
version = "..."
features = ["load:sync"]
```

```rust
use figleaf::prelude::*;

#[figleaf]
#[derive(Deserialize)]
struct Config { etc: bool }

let builder = Config::build()
  .basename("parka")
  .languages(&[Language::TOML, Language::JSON])
  .finalise_sync()?;
```

It is recommended to disable default features and only using this loading
feature in this case, to cut down dependencies and code size / compile time.

### Singleton

A common pattern is to load config at program start, put it in a static, and
have it be available to all parts of your program without passing a variable
around. Figleaf's singleton support streamlines this, and comes in three
variants.

In all cases, the default `auto()` builder is used unless a builder function is
provided:

```rust
use figleaf::prelude::*;

#[figleaf(singleton = "eager", singleton_builder = builder)]
#[derive(Deserialize)]
struct Config { etc: bool }

fn builder() -> figleaf::Builder<Config> {
  Config::build()
    .basename("polo")
}
```

#### Static config, loaded eargerly

```rust
use figleaf::prelude::*;

#[figleaf(singleton = "eager")]
#[derive(Deserialize)]
struct Config { etc: bool }

dbg!(Config::singleton().etc);
```

This variant loads at program start, without needing a hook in `main`. Errors
encountered while loading panic.

#### Static config, loaded lazily

```rust
use figleaf::prelude::*;

#[figleaf(singleton = "lazy")]
#[derive(Deserialize)]
struct Config { etc: bool }

dbg!(Config::singleton().etc);
```

This variant is like the eager one, but does nothing at program start, and is
instead loaded on the first access.

#### Dynamic config, in main

If more context is needed to create the builder, or error handling is required,
the singleton can be created from your own code (i.e. in main):

```rust
use figleaf::prelude::*;

#[figleaf(singleton = "lazy")]
#[derive(Deserialize)]
struct Config { etc: bool }

fn main() -> Result<Box<dyn std::error::Error>> {
  let args = get_some_args()?;
  Config::build().basename(args.config_name).as_singleton_sync()?;

  dbg!(Config::singleton().etc);
}

dbg!(Config::singleton().etc);
```

The `as_singleton` method supports the same loading features as the
`finalise()` method (here shown with `load:sync`).

## Recursive reconfiguration

Recursive reconfiguration is one of our flagship advanced features. To
understand what it does, consider the simple case of an environment variable
that dictates what the configuration files are named: in a traditional system,
this would either need two separate systems (loading and parsing the
environment, and loading the configuration), loading the configuration twice
based on a condition, or some other manual process.

Here, instead, as long as you tell Figleaf about it, it will take care of
recursively loading configuration, checking reconfiguration fields, and
adjusting until done.

This makes possible complex scenarios, such as (all subject to configuration,
none of these are Figleaf default strategies):

- changing the prefix of environment variables from an environment variable;
- specifying a `DATABASE_URL` to enable loading the rest of the config from a
  database table;
- a `zeroconfig_ns=` key in a TOML file points to a Zeroconfig namespace on the
  local network where a configuration server listening on a TCP port advertises
  its address;
- the PCI address of a hardware token is provided via a blob concatenated to
  the binary, which is used to decrypt the rest of the concat'd blob into
  configuration secrets; meanwhile the innoffensive part of the config is
  loaded from plain text file as per normal.

TODO: usage

## In libraries

Figleaf can be used in libraries, not just top-level applications.

If a library needs configuration, in addition to offering a configuration
struct to its consumers to be completed at their discretion, it can request
configuration via Figleaf, with the same power features as usual Figleaf usage.

The difference is that the top-level application always can have control if it
so decides. It can discover configuration requests, and intercept, redirect,
remap, override... them. If the top-level application either doesn't use
Figleaf or doesn't interfere with library configuration requests, things
proceed as usual.

### TODO: the rest

## Languages

Configuration languages are used to parse files and file-like inputs. Figleaf
works exclusively with Serde-implemented languages (Serde calls them
“formats”). There are several ways to select which languages are available, and
which is used.

### The default set

- [JSON](https://en.wikipedia.org/wiki/JSON). Feature: `lang:json`.
- [TOML](https://en.wikipedia.org/wiki/TOML). Feature: `lang:toml`.
- [YAML](https://en.wikipedia.org/wiki/YAML). Feature: `lang:yaml`.
- [Dhall](https://dhall-lang.org). Feature: `lang:dhall`.

JSON, TOML, and YAML are the most used configuration languages.

Dhall is the author’s hope for a better configuration standard.

### Compile-time selection

All built-in languages are feature-gated (with the default set being included
in the crate’s default features), all denoted by `lang:` as demonstrated above.

The non-default built-in languages are:

- [Human JSON](https://hjson.org). Feature: `lang:hjson`.
- [HOCON](https://github.com/lightbend/config/blob/master/HOCON.md). Feature: `lang:hocon`.
- [INI](https://en.wikipedia.org/wiki/INI_file). Feature: `lang:ini`.
- [JSON5](https://json5.org). Feature: `lang:json5`.
- [MuON](https://github.com/muon-data/muon). Feature: `lang:muon`.
- [PHP `serialize`](https://www.php.net/manual/en/function.serialize.php). Feature: `lang:php`.
- [RON](https://github.com/ron-rs/ron). Feature: `lang:ron`.
- [S-expressions using `lexpr`](https://github.com/rotty/lexpr-rs). Feature: `lang:lexpr`.
- [XML](https://en.wikipedia.org/wiki/XML). Feature: `lang:xml`.

The (not recommended apart from prototyping and testing) `lang:all` feature
includes all supported built-in languages.

### Using additional languages

Using more languages than the built-in ones is possible: all that's needed is a
serde format library, a conversion from its `Value` type to Figleaf’s, and an
extension/mime-type list.

TODO: example

### Runtime selection

By default, all languages compiled-in are used. However, selecting languages
can be done at runtime.

TODO: example

### File extensions and mimetypes

Each language has one or more associated file extensions and one or more IANA
media type (commonly known as “mimetype”). The built-in ones are:

|    Language    | Extensions            | Media types |
|:--------------:|:----------------------|:------------|
| Human JSON     | `hjson`               | `application/hjson` |
| HOCON          | `hocon`, `hoconf` [1] | `application/hocon` |
| INI            | `ini`                 | `application/textedit`, `zz-application/zz-winassoc-ini` |
| JSON           | `json`                | `application/json` |
| JSON5          | `json5`               | `application/json5` |
| MuON           | `muon`                | (none) |
| PHP’s serialize| (none)                | (none) |
| RON            | `ron`                 | (none) |
| S-expressions  | (none)                | (none) |
| TOML           | `toml`                | `application/toml` |
| XML            | `xml`                 | `application/xml` |
| YAML           | `yaml`, `yml`         | `application/yaml`, `application/x-yaml`, `text/yaml` |

<sup>
[1] HOCON’s default is `.conf`, but that is supremely ambiguous.
You can still opt-in to this usage with `.add_extension(Language::Hocon, "conf")`.
</sup>

Extensions are used when reading files, media types when reading streams with
an indicated content type or when detecting content type e.g. with a “magic”
database. It’s always possible to add or override the extensions and media
types for a language:

```rust
builder
  .add_extension(Language::YAML, "yuml")
  .set_mediatypes(Language::YAML, &["application/yuml"])
```

### Auto-detection

For streams, files, or file-like sources without a content type indication or
extension, type can be auto-detected. There are three ways to achieve this:

1. Trying all enabled formats in turn until one doesn’t error. This is the
   default, as it is easiest, even though it is innefficient and may result in
   false-positives and errors.

2. Using a “magic” database. Enabled with the `magic-detect` feature. The
   general principle is that the first few hundred bytes of a file are read and
   a series of heuristics are applied. This returns a media type, which is used
   as defined above. In case nothing is found, falls back to 1.

3. Providing a function that takes the source information and returns a
   `Language::` enum variant or `None`. Falls back to 1 (or 2 if enabled).

## Sources

Most of the sections below deal with the various kinds of configuration sources
that Figleaf supports, either out of the box or as features. Sources are where
configuration lives and is pulled from into your program.

### Features

Each source has its corresponding feature namespaced under `source:`. To see the
full list of source features, check out [the appendix].

### Filtering

By default, if a source is compiled in (if its feature is enabled), it is used.
Source filtering takes a [Pattern] and matches on source names: positive matches
are allowed. For security-sensitive applications, source safelisting may be
important, though disabling sources at compilation time is likely better.

Source filtering can also be used in recursive reconfiguration scenarios, with
a configuration option enabling more configuration sources.

TODO: example

### Dynamic

Enabling a source via its feature increases compilation time and bloats the
compiled size of the program. While it would be great to always support all
kinds of sources so end-users have the greatest flexibility, this is often
impractical. Figleaf attempts to mitigate this via shared libraries.

Support is enabled by default and toggled with the `source:dynamic` feature.
This means that if [the `libfigleaf.so` or `ligfigleaf.dll` library][libfig] is
available at a standard system location, Figleaf will load it and enable every
source available via that library, unless source filtering has been enabled.

The [`default:minimal` feature profile] heavily relies on dynamic source
support: along with other optimisations, it enables _only_ environment as a
built-in source, and delegates all other sources to the shared library.

Not all sources are available dynamically, and not all of these are compiled in
[the pre-built Figleaf library distribution][libfig].

You can configure the library path and name, and you can compile your own
Figleaf shared library with different features enabled.

TODO: example

TODO: [libfig] link

