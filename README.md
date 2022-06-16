# tide-compress

Outgoing compression middleware for the [Tide][] server framework.

```rust
#[async_std::main]
async fn main() -> tide::Result {
    let mut app = tide::new();
    app.with(tide_compress::CompressMiddleware::new());
}
```

## Features

- Support for Brotli, Gzip, and Deflate encodings, compile-time configurable through cargo feature flags.
  - Prioritizes Brotli if available.
  - Only pulls in the necessary dependencies for the desired configuration.
  - Defaults to Brotli & Gzip.
- `Accept-Encoding` checking including priority.
- Minimum body size threshold.
  - Configurable when created by `CompressMiddleware::with_threshold(usize)`.
- Does not compress responses with a [`Cache-Control: no-transform`](https://developer.mozilla.org/en-US/docs/Web/HTTP/Headers/Cache-Control) header.
- Sets the [`Vary`](https://developer.mozilla.org/en-US/docs/Web/HTTP/Headers/Vary) header.
- Checks the [`Content-Type`](https://developer.mozilla.org/en-US/docs/Web/HTTP/Headers/Content-Type) header (MIME) against a regular expression, by default: `^text/|\+(?:json|text|xml)$` (case insensitive).
  - Fully override-able to any custom [`Regex`](https://docs.rs/regex/1/regex/struct.Regex.html), with `None` as an option.
  - Functionality can be excluded in crate features if the `regex` crate poses build issues.

## Caveats

- Still missing advanced `Content-Type` (MIME) checking.

## License

Licensed under the [BlueOak Model License 1.0.0](LICENSE.md) — _[Contributions via DCO 1.1](contributing.md#developers-certificate-of-origin)_

[Tide]: https://github.com/http-rs/tide

