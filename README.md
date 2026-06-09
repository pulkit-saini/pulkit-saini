name: Generate Arkanoid Contribution Game

on:
  schedule:
    - cron: "0 */6 * * *"   # runs every 6 hours
  workflow_dispatch:          # allows manual trigger from Actions tab
  push:
    branches:
      - main

jobs:
  build:
    runs-on: ubuntu-latest
    timeout-minutes: 10

    steps:
      - name: Generate Arkanoid SVG (light)
        uses: Platane/snk/svg-only@v3
        with:
          github_user_name: ${{ github.repository_owner }}
          outputs: |
            dist/arkanoid.svg?color_snake=7C83FD&color_dots=#0d1117,#1a1f35,#302b63,#534AB7,#7C83FD&color_empty=#0d1117

      - name: Generate Arkanoid SVG (dark)
        uses: Platane/snk/svg-only@v3
        with:
          github_user_name: ${{ github.repository_owner }}
          outputs: |
            dist/arkanoid-dark.svg?color_snake=FF6B6B&color_dots=#161b22,#1a2535,#253356,#3d5a8a,#7C83FD&color_empty=#161b22

      - name: Push output to branch
        uses: crazy-max/ghaction-github-pages@v3
        with:
          target_branch: output
          build_dir: dist
        env:
          GITHUB_TOKEN: ${{ secrets.GITHUB_TOKEN }}
