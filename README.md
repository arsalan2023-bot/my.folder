name: CI

on:
  push:
    branches: ["**"]
  pull_request:
    branches: ["**"]

permissions:
  contents: read

jobs:
  build-test:
    runs-on: ubuntu-lates

    steps:
      - name: Checkout
        uses: actions/checkout@v4

      # -------- Node.js (اختیاری: فقط اگر package.json وجود داشته باشد) --------
      - name: Set up Node.js
        if: hashFiles('package.json') != ''
        uses: actions/setup-node@v4
        with:
          node-version: 'lts/*'
          cache: npm

      - name: Install Node deps
        if: hashFiles('package.json') != ''
        run: npm ci

      - name: Run Node tests
        if: hashFiles('package.json') != ''
        run: |
          if npm run | grep -q " test"; then
            npm test
          else
            echo "No npm test script; skipping."
          fi

      - name: Prettier check (Node)
        if: hashFiles('package.json') != ''
        run: |
          if npm run | grep -q " prettier:check"; then
            npm run prettier:check
          else
            echo "Tip: add script \"prettier:check\": \"prettier -c .\""
          fi

      # -------- Python (اختیاری: فقط اگر pyproject.toml یا requirements.txt وجود داشته باشد) --------
      - name: Set up Python
        if: hashFiles('pyproject.toml') != '' || hashFiles('requirements.txt') != ''
        uses: actions/setup-python@v5
        with:
          python-version: '3.x'
          cache: 'pip'

      - name: Install Python deps
        if: hashFiles('pyproject.toml') != '' || hashFiles('requirements.txt') != ''
        run: |
          python -m pip install --upgrade pip
          if [ -f "pyproject.toml" ]; then
            # ترجیح به نصب از pyproject (poetry/pdm/hatch)
            if grep -q "\[tool.poetry\]" pyproject.toml; then
              pip install poetry && poetry install --no-interaction
            elif grep -q "\[tool.pdm\]" pyproject.toml; then
              pip install pdm && pdm install
            elif grep -q "\[project\]" pyproject.toml; then
