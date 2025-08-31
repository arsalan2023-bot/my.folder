name: Hello World

on: [push, pull_request]

jobs:
  say-hello:
    runs-on: ubuntu-latest
    steps:
      - name: Checkout repo
        uses: actions/checkout@v4

      - name: Say Hello

