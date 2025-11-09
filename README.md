name: Multi-Platform Matrix Build (CA5F892)

on:
  push:
    branches: [ main ]
  pull_request:

jobs:
  build:
    strategy:
      matrix:
        node-version: [14, 16, 18]  # three variants
    runs-on: ubuntu-latest

    steps:
      - name: Checkout code
        uses: actions/checkout@v4

      - name: Setup Node.js ${{ matrix.node-version }}
        uses: actions/setup-node@v4
        with:
          node-version: ${{ matrix.node-version }}

      - name: matrix-ca5f892
        run: |
          echo "Building for Node.js version ${{ matrix.node-version }}"

      - name: Generate build artifact
        run: |
          ART=output-${{ matrix.node-version }}.txt
          echo "Build output for Node.js version ${{ matrix.node-version }}" > $ART
          echo "Generated on: $(date)" >> $ART

      - name: Upload artifact
        uses: actions/upload-artifact@v4
        with:
          name: build-ca5f892-node${{ matrix.node-version }}
          path: output-${{ matrix.node-version }}.txt
