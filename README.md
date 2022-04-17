# Überauth Spotify

[![License](https://img.shields.io/hexpm/l/ueberauth_spotify.svg)](https://github.com/ueberauth/ueberauth_spotify/blob/master/LICENSE)

> Spotify OAuth2 strategy for Überauth.

## Installation

1. Setup your application at [Spotify Developers Dashboard](https://developer.spotify.com/dashboard/).

2. Add `:ueberauth_spotify` to your list of dependencies in `mix.exs`:

   ```elixir
   def deps do
     [
       {:ueberauth_spotify, "~> 0.1"}
     ]
   end
   ```

3. Add Spotify to your Überauth configuration:

   ```elixir
   config :ueberauth, Ueberauth,
     providers: [
       spotify: {Ueberauth.Strategy.Spotify, []}
     ]
   ```

4. Update your provider configuration:

   ```elixir
   config :ueberauth, Ueberauth.Strategy.Spotify.OAuth,
     client_id: System.get_env("SPOTIFY_CLIENT_ID"),
     client_secret: System.get_env("SPOTIFY_CLIENT_SECRET")
   ```

5. Include the Überauth plug in your controller:

   ```elixir
   defmodule MyApp.AuthController do
     use MyApp.Web, :controller
     plug Ueberauth
     ...
   end
   ```

6. Create the request and callback routes if you haven't already:

   ```elixir
   scope "/auth", MyApp do
     pipe_through :browser

     get "/:provider", AuthController, :request
     get "/:provider/callback", AuthController, :callback
   end
   ```

7. Your controller needs to implement callbacks to deal with `Ueberauth.Auth` and `Ueberauth.Failure` responses.

For an example implementation see the [Überauth Example](https://github.com/ueberauth/ueberauth_example) application.

## Calling

Depending on the configured URL you can initialize the request through:

    /auth/spotify

Or with options (`auth_type`, `scope`, `locale`, `display`):

    /auth/spotify?scope=user-read-email

By default the requested scope is "user-read-email". Scope can be configured either explicitly as a `scope` query value on the request path or in your configuration:

```elixir
config :ueberauth, Ueberauth,
  providers: [
    spotify: {Ueberauth.Strategy.Spotify, [default_scope: "user-read-email,playlist-modify-public"]}
  ]
```

## Copyright and License

Copyright (c) 2015 Sean Callan

Released under the MIT License, which can be found in the repository in [LICENSE](./LICENSE).
