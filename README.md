on:
  push:
    branches:
      - main  # Trigger on pushes to the main branch
  pull_request:
    branches:
      - main  # Trigger on PRs targeting the main branch

jobs:
  build:
    runs-on: ubuntu-latest  # Fixed "ubunty" typo
    steps:
      - uses: actions/checkout@v4
      - name: Run a build script
        run: echo "Building your project..."
