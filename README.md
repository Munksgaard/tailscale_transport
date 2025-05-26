# TailscaleTransport

A transport for ThousandIsland that allows exposing services directly to your tailnet.

An example application can be found in [tschat](https://github.com/Munksgaard/tschat).

In addition to the transport itself, this package also provides an
authentication plug `TailscaleTransport.Plug` which gets information about
connecting clients using Tailscales LocalAPI.

This module is based in the [`libtailscale` NIF
wrapper](https://hex.pm/packages/libtailscale) and
[`gen_tailscale`](https://hex.pm/packages/gen_tailscale).

Everything in this chain of packages should be considered proof of concept at
this point and should not be used for anything important. Especially
`gen_tailscale`, which has been constructed by crudely hacking the original
`gen_tcp` module to use `libtailscale` and could use a total rewrite at some
point. However, it works well enough that my example application
[`tschat`](https://github.com/Munksgaard/tschat) is able to accept connections
from different Tailscale users and show their username by retrieving data from
the Tailscale connection.


## Installation

The package can be installed by adding `tailscale_transport` to your list of
dependencies in `mix.exs`:

```elixir
def deps do
  [
    {:tailscale_transport, "~> 0.1.0"}
  ]
end
```

## Usage

In your `config/config.exs`, when defining your endpoint, specify that you want
to use the `TailscaleTransport` module as the transport module for
`ThousandIsland`:

```elixir
config :my_app, MyAppWeb.Endpoint,
  url: [host: "my-app"],
  adapter: Bandit.PhoenixAdapter,
  ...
  http: [
    thousand_island_options: [
      transport_module: TailscaleTransport,
      transport_options: [hostname: "my-app"]
    ]
  ]
```

Notice that you need to specify the same hostname in the `url` parameter and as
the `hostname` parameter to the `transport_options` option.
