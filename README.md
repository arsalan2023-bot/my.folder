name: Hello World

on: [push, pull_request]

jo:
  say-hello:
    runs-on: ubuntu-latest
    steps:
      - name: Checkout repo
        uses: actions/checkout@v4

      - name: Say Hello

