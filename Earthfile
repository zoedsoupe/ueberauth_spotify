# App versions
ARG ELIXIR="1.13.4"
ARG OTP="24.1.1"
ARG ALPINE="3.15.0"

all:
  BUILD +format
  BUILD +credo
  BUILD +unit-test

format:
  FROM +test-setup

  RUN mix format --check-formatted

credo:
  FROM +test-setup

  RUN mix credo --strict

unit-test:
  FROM +test-setup

  RUN mix test --only unit

setup-base:
  FROM hexpm/elixir:$ELIXIR-erlang-$OTP-alpine-$ALPINE

  RUN apk add --no-progress --update git build-base
  ENV ELIXIR_ASSERT_TIMEOUT=10000
  WORKDIR /src

test-setup:
  FROM +setup-base

  COPY mix.exs mix.lock .formatter.exs .
  COPY --dir config lib priv test .

  RUN mix local.rebar --force --if-missing
  RUN mix local.hex --force --if-missing

  ENV MIX_ENV="test"

  RUN mix deps.get
  RUN mix compile

  SAVE ARTIFACT _build ./_build
