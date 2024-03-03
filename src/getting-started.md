To use faer in your project, you can add the required dependency to your `Cargo.toml` file.
```toml
[dependencies]
faer = "0.18"
```

For day-to-day development, we recommend enabling optimizations for `faer`, since the layers of generics can add considerable overhead in unoptimized builds.
This can be done by adding the following to `Cargo.toml`
```toml
[profile.dev.package.faer]
opt-level = 3
```

We recommend new users to skim the user guide provided in the sidebar, and defer to the docs for more detailed information.
