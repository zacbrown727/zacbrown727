name: CI/CD Pipeline

on:
  push:
    branches:
      - main  # Trigger on pushes to the main branch
  pull_request:
    branches:
      - main  # Trigger on PR to the main branch

jobs:
  build:
    runs-on: ubuntu-latest

    steps:
      - name: Checkout code
        uses: actions/checkout@v2

      - name: Set up Node.js
        uses: actions/setup-node@v2
        with:
          node-version: '14'  # Specify the Node.js version

      - name: Install dependencies
        run: npm install

      - name: Run tests
        run: npm test

      - name: Build the project
        run: npm run build

  deploy:
    runs-on: ubuntu-latest
    needs: build
    if: github.ref == 'refs/heads/main'

    steps:
      - name: Checkout code
        uses: actions/checkout@v2

      - name: Deploy to Server
        env:
          API_URL: ${{ secrets.API_URL }}
          API_TOKEN: ${{ secrets.API_TOKEN }}
        run: |
          curl -X POST $API_URL/deploy \
          -H "Authorization: Bearer $API_TOKEN" \
          -d '{"status": "deploying", "timestamp": "$(date)"}'
