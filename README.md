name: Hello World

on: [push, pull_request]

job:
  say-hello:
    runs-on: ubuntu-lates
    steps:
      - name: Checkout repo
        uses: actions/checkout@v4

      - name: Say Hello

