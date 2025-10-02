name: CI Test

on: [push]

jobs:
  buld:
    runs-on: ubuntu-latest
    steps:
      - name: Checkout repo
        uses: actions/checkout@v4

      - name: Run Hello Script
        run: echo "✅ GitHub Actions اجرا شد!"
