# How to Build:

```sh
gwee render . && npx local-web-server --https --directory public
```

Check that everything works, then:

```bash
#!/usr/bin/env bash

export BUILD_DIR="public"
export RELEASE_BRANCH="gh-pages"

git fetch origin $RELEASE_BRANCH || git checkout -b $RELEASE_BRANCH
git add -f $BUILD_DIR
tree=$(git write-tree --prefix=$BUILD_DIR)
git reset -- $BUILD_DIR
export identifier=$(git describe --dirty --always)

git push -u origin $RELEASE_BRANCH
export commit=$(git commit-tree -p refs/remotes/origin/$RELEASE_BRANCH -m "Deploy $identifier" $tree)
git update-ref refs/heads/$RELEASE_BRANCH $commit
git push origin refs/heads/$RELEASE_BRANCH
git checkout main
```