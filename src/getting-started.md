to use faer in your project, you can add the required dependency to your `cargo.toml` file.
```toml
[dependencies]
faer = "0.19"
```

for day-to-day development, we recommend enabling optimizations for _`faer`_, since the layers of generics can add considerable overhead in unoptimized builds.
this can be done by adding the following to `Cargo.toml`
```toml
[profile.dev.package.faer]
opt-level = 3
```

we recommend new users to skim the user guide provided in the sidebar, and defer to the docs for more detailed information.
