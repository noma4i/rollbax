# Rollbax - Fork 
## This version differs from original Rollbax

Elixir client for [Rollbar](https://rollbar.com).

## Installation

Add Rollbax as a dependency to your `mix.exs` file:

```elixir
defp deps() do
  [
    {:rollbax, "~> 0.6", github: "noma4i/rollbax"}
  ]
end
```

and add it to your list of applications:

```elixir
def application() do
  [
    applications: [
      :rollbax
    ]
  ]
end
```

Then run `mix deps.get` in your shell to fetch the dependencies.

## Usage

Rollbax requires some configuration in order to work. For example, in `config/config.exs`:

```elixir
config :rollbax,
  access_token: "ffb8056a621f309eeb1ed87fa0c7",
  environment: "production"
```

Then, exceptions (errors, exits, and throws) can be reported to Rollbar using `Rollbax.report/3`:

```elixir
try do
  DoesNotExist.for_sure()
rescue
  exception ->
    Rollbax.report(:error, exception, System.stacktrace())
end
```

For detailed information on configuration and usage, take a look at the [online documentation](http://hexdocs.pm/rollbax).

### Logger backend

Rollbax provides a backend for Elixir's `Logger` as the `Rollbax.Logger` module. It can be configured as follows:

```elixir
# We register Rollbax.Logger as a Logger backend.
config :logger,
  backends: [Rollbax.Logger]

# We configure the Rollbax.Logger backend.
config :logger, Rollbax.Logger,
  level: :error
```

Sending logged messages to Rollbar can be disabled via `Logger` metadata:

```elixir
# To disable reporting for all subsequent logs:
Logger.metadata(rollbar: false)

# To disable reporting for the current logged message only:
Logger.error("oops", rollbar: false)
```

### Plug and Phoenix

*usage goes here*

### Ignore errors

Rollbax can ignore errors by their Exception module name. For example, to ignore 404s in Phoenix, you can use the following config option

```elixir
config :rollbax, ignore_list: [Phoenix.Router.NoRouteError]
```

### Non-production reporting

For non-production environments error reporting can be either disabled completely (by setting `:enabled` to `false`) or replaced with logging of exceptions (by setting `:enabled` to `:log`).

```elixir
config :rollbax, enabled: :log
```

## License

This software is licensed under [the ISC license](LICENSE).
