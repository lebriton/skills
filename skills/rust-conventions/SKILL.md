---
name: rust-conventions
description: 'Rust code conventions. Use when writing or reviewing Rust code.'
---

# Rust Conventions

Every new struct and enum follow all three conventions below: canonical derive ordering, separated impl blocks, and alphabetical method order.

## Derive Ordering

When adding `#[derive(...)]` to a struct or enum, use canonical order: standard Rust traits first, then all additional traits alphabetically.

```rust
#[derive(Copy, Clone, Eq, PartialEq, Ord, PartialOrd, Hash, Debug, Display, Default)]
struct Config {
    name: String,
    version: u32,
}

#[derive(Copy, Clone, Eq, PartialEq, Hash, Debug, Deserialize, Serialize)]
struct RawConfig {
    data: Vec<u8>,
}
```

## Impl Blocks

When a struct has multiple impl blocks, separate them by purpose: constructors first, then accessors and business logic, then one block per trait implementation.

```rust
impl Config {
    pub fn new(name: &str) -> Self { todo!() }
    pub fn with_version(name: &str, version: u32) -> Self { todo!() }
}

impl Config {
    pub fn name(&self) -> &str { todo!() }
    pub fn save(&self) -> Result<()> { todo!() }
}

impl Display for Config { todo!() }
impl From<Config> for RawConfig { todo!() }
```

## Method Ordering

When writing methods within an impl block, sort them alphabetically.

```rust
impl Config {
    pub fn name(&self) -> &str { todo!() }
    pub fn save(&self) -> Result<()> { todo!() }
    pub fn validate(&self) -> bool { todo!() }
}
```
