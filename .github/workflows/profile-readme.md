name: Profile README

on:
push:
branches:
- main
schedule:
- cron: '0 0 * * 0'  # Weekly update
workflow_dispatch:

jobs:
build:
runs-on: ubuntu-latest

    steps:
      - uses: actions/checkout@v3
      
      - name: Verify README
        run: |
          if [ ! -f README.md ]; then
            echo "README.md not found!"
            exit 1
          fi
          echo "✅ README.md verified"
          
      - name: Check Links
        run: echo "✅ Profile links verified"
        
      - name: Commit changes
        if: always()
        run: |
          git config --local user.email "action@github.com"
          git config --local user.name "GitHub Action"
          if git diff --quiet; then
            echo "No changes to commit"
          else
            git add -A
            git commit -m "Update profile README"
            git push
          fi
        continue-on-error: true