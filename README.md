name: CI Test

on: [push]

jos:
  build:
    runs-on: ubuntu-lates
    steps:
      - name: Checkout repo
        uses: actions/checkout@v4

      - name: Run Hello Script
        run: echo "✅ GitHub Actions اجرا شد!"
