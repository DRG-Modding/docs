# Contributing

The handbook is written in [Markdown] and is rendered via [mdBook]. Please see
[mdBook's usage guide][mdBook] for how to use [mdBook] locally to render your documentation.
CI is setup to automatically deploy from `master`.

## mdBook Basics

Precompiled [mdBook] binaries are available at <https://github.com/rust-lang/mdBook/releases>.

You can also install [mdBook] via [cargo] after you [install Rust][rust]:

```bash
cargo install mdbook
```

You can watch the file changes and preview the rendered HTML files via

```bash
mdbook serve --open
```

[Markdown]: https://www.markdownguide.org/
[mdBook]: https://rust-lang.github.io/mdBook/index.html
[cargo]: https://doc.rust-lang.org/cargo/
[rust]: https://www.rust-lang.org/tools/install
