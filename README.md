# sharingData

Here's the command to display both the revision and deleted files:
git log --diff-filter=D --format="%h %d %s (%an, %ar)" --name-only --no-renames
