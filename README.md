# (work-in-progress) `cargo clippy-steal`

With this command, you can:

- Include other clippy configurations in your own `Cargo.toml` by specifying a URL to download from
- Your custom configs will be merged, and will take precedence

I want to steal `iced` clippy configuration. To do this, in my `Cargo.toml` I'll add this:

```toml
type-complexity = "deny"
#:: <clippy-steal>
#:: url = https://raw.githubusercontent.com/iced-rs/iced/e462535f3ba0bc1cdaa4a101b48542816f777e53/Cargo.toml
#:: sha256 = b6b162c13254b9fc2705d3a1cb22c2e14f97223f9684b27acd5411b9a91fadda
#:: - workspace.lints.clippy -> lints.clippy
#:: - workspace.lints.rustdoc

#:: </clippy-steal>
```

`iced`'s clippy config will be downloaded and inserted in-between the `<clippy-steal>` tag like this:

```toml
type-complexity = "deny"

#:: <clippy-steal>
#:: url = https://raw.githubusercontent.com/iced-rs/iced/e462535f3ba0bc1cdaa4a101b48542816f777e53/Cargo.toml
#:: sha256 = b6b162c13254b9fc2705d3a1cb22c2e14f97223f9684b27acd5411b9a91fadda
#:: - workspace.lints.clippy -> lints.clippy
#:: - workspace.lints.rustdoc

[lints.clippy]
# type-complexity = "allow"
map-entry = "allow"
semicolon_if_nothing_returned = "deny"

[workspace.lints.rustdoc]
broken_intra_doc_links = "forbid"

#:: </clippy-steal>
```

Since we already had `type-complexity`, it overrides `iced`'s clippy config.

## Future

Support `.rustfmt.toml` and `.clippy.toml`, too.
