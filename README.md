name: Multi-Platform Matrix Build with Artifacts

on:
  push:
    branches:
      - main
  workflow_dispatch:

jobs:
  matrix_build:
    # Use a clean base OS for all builds
    runs-on: ubuntu-latest
    
    # Define the matrix strategy with three variants (Node.js versions)
    strategy:
      fail-fast: false
      matrix:
        node-version: [18, 20, 22] # Our three required variants
    
    steps:
      - name: Checkout Repository
        uses: actions/checkout@v4

      - name: Setup Node.js ${{ matrix.node-version }}
        uses: actions/setup-node@v4
        with:
          node-version: ${{ matrix.node-version }}

      # Required Step Identifier for validation
      - name: matrix-1bf3641 - Running Build for Node v${{ matrix.node-version }}
        run: |
          echo "Starting simulated build for Node.js version ${{ matrix.node-version }}..."
          # Simulate install dependencies
          # npm install
          
      - name: Generate unique build artifact
        run: |
          BUILD_CONTENT="This is the artifact output generated on $(date) for Node.js v${{ matrix.node-version }}."
          echo "$BUILD_CONTENT" > build-output-${{ matrix.node-version }}.txt
        
      # Upload the artifact with the required naming prefix
      - name: Upload Artifact
        uses: actions/upload-artifact@v4
        with:
          # Required prefix and variant for validation
          name: build-1bf3641-node-${{ matrix.node-version }}
          path: build-output-${{ matrix.node-version }}.txt
          retention-days: 1
