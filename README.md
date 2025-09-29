name: CI Test

on: [push]

jobs:
  build:
    runs-on: ubuntu-latest
    steps:
      - name: Checkout rep
        uses: actions/checkout@v4

      - name: Run Hello Script
        run: echo "✅ GitHub Actions اجرا شد!"
