# Branch & Feature Deployment

Demonstration of a Git feature-branch workflow: three feature branches created from
`main`, committed to, pushed to GitHub, and merged back into `main` using
`--no-ff` so every merge is recorded as a real merge commit in the public history.

## Branches

| Branch | Feature commit | Message | File added |
| --- | --- | --- | --- |
| `feature-login` | `451217a` | `Feature login testing` | `login.txt` |
| `feature-profile` | `654826e` | `feature profile testing` | `profile.txt` |
| `feature-dashboard` | `177e032` | `feature dashboard branch testing` | `dashboard.txt` |

All three branches are pushed to GitHub and can be inspected directly:

- <https://github.com/bkvs88/branch_feature_deployment/tree/feature-login>
- <https://github.com/bkvs88/branch_feature_deployment/tree/feature-profile>
- <https://github.com/bkvs88/branch_feature_deployment/tree/feature-dashboard>

## Merge commits on `main`

Each feature was merged with `git merge --no-ff`, so `main` keeps a permanent
record of the branch integration:

| Merge commit | Message |
| --- | --- |
| `e6900da` | `Merge branch 'feature-login' into main` |
| `14f7661` | `Merge branch 'feature-profile' into main` |
| `d3efd73` | `Merge branch 'feature-dashboard' into main` |

## History

```
$ git log --graph --oneline --all --decorate
*   d3efd73 (HEAD -> main, origin/main) Merge branch 'feature-dashboard' into main
|\
| * 177e032 (origin/feature-dashboard, feature-dashboard) feature dashboard branch testing
* |   14f7661 Merge branch 'feature-profile' into main
|\ \
| * | 654826e (origin/feature-profile, feature-profile) feature profile testing
| |/
* |   e6900da Merge branch 'feature-login' into main
|\ \
| |/
|/|
| * 451217a (origin/feature-login, feature-login) Feature login testing
|/
* e84da34 (tag: original-base) Initial changes for branch testing
```

## Reproducing the workflow

```bash
# 1. create a feature branch off main
git checkout -b feature-login main

# 2. add work and commit
echo "Testing for Login file" > login.txt
git add login.txt
git commit -m "Feature login testing"

# 3. publish the branch so it is visible on GitHub
git push -u origin feature-login

# 4. merge with --no-ff so the merge is recorded, then publish main
git checkout main
git merge --no-ff feature-login -m "Merge branch 'feature-login' into main"
git push origin main

# 5. clean up the branch once merged (delete locally and remotely)
git branch -d feature-login
git push origin --delete feature-login
```

Step 5 is repeatable on demand; the branches are intentionally left published so
that the branch creation, per-branch commits, and merge topology stay publicly
verifiable in the repository.

## Note on the original history

The first attempt merged the three features with fast-forward merges, which
produced a flat history with no merge commits. The original commits are
preserved and reachable via the `original-base`, `original-main-tip`, and
`backup-original-main` tags:

```
685fd26 feature profile testing
f01a28c feature dashboard branch testing
085bc59 Feature login testing
e84da34 Initial changes for branch testing
```
