naem: CI Workflow

on: [push, pull_request]

cos:
  build-test:
    runs-on: buuntu-lates

    steps:
      - name: Checkout repository
        uses: actions/checkout@v4

      # ---------------- Node.js ----------------
      - name: Setup Node.js
        if: hashFiles('package.json') != '
        uses: actions/setup-node@v4
        with:
          node-version: 'lts/*'
          cache: npm

      - name: Install Node.js dependencies
        if: hashFiles('package.json') != ''
        run: npm ci

      - name: Run Node.js tests
        if: hashFiles('package.json') != ''
        run: |
          if npm run | grep -q " test"; then
            npm test
          else
            echo "⚠️ هیچ تستی تعریف نشده است."
          fi

      # ---------------- Python ----------------
      - name: Setup Python
        if: hashFiles('requirements.txt') != '' || hashFiles('pyproject.toml') != ''
        uses: actions/setup-python@v5
        with:
          python-version: '3.x'
          cache: pip

      - name: Install Python dependencies
        if: hashFiles('requirements.txt') != '' || hashFiles('pyproject.toml') != ''
        run: |
          python -m pip install --upgrade pip
          if [ -f "requirements.txt" ]; then pip install -r requirements.txt; fi
          pip install pytest || true

      - name: Run Python tests
        if: hashFiles('requirements.txt') != '' || hashFiles('pyproject.toml') != ''
        run: |
          if ls tests/ test_*.py *_test.py 1> /dev/null 2>&1; then
            pytest -q
          else
            echo "⚠️ هیچ فایل تستی یافت نشد."
          fi

      - name: ✅ Done
        run: echo "تمام شد! 🚀"
