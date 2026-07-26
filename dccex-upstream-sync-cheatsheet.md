# Syncing DCC-EX Upstream Updates — Cheat Sheet

For pulling upstream bugfixes/releases into `sbegno_SPKPRR_CS_Build` without losing custom work (RS485 bus, SRESERVE, etc.)

## The three things to remember

- **`upstream`** = official DCC-EX repo (read-only reference)
- **`origin`** = your GitHub fork
- **`sbegno_SPKPRR_CS_Build`** = your real working branch — this is where your work lives
- **`master`** = never worked on directly; just a mirror of `upstream/master` used as a clean staging point

## The workflow (every time DCC-EX releases an update)

```bash
# 1. Get the latest info from upstream (no files change yet)
git fetch upstream

# 2. Peek at what's new before doing anything
git log master..upstream/master --oneline

# 3. Update your local master to match upstream
git checkout master
git merge upstream/master
# should always be a clean fast-forward — you never commit on master directly

# 4. Switch to your real working branch
git checkout sbegno_SPKPRR_CS_Build

# 5. Bring the updates into your branch
git merge master
```

## Step 5 outcomes

**Clean merge** → a commit message editor opens, save & close it, done.

**Conflicts** → Git lists the conflicting files and marks the spots inside them:
```
<<<<<<< HEAD
your version
=======
upstream version
>>>>>>> master
```
Edit each spot to keep what you want (delete the marker lines too), then:
```bash
git add <fixed files>
git commit
```

## Push to GitHub when you're ready

```bash
git push origin master
git push origin sbegno_SPKPRR_CS_Build
```

## Habits worth keeping

- Always run step 2 first — free info on whether the update even touches files you care about.
- If `git log master..upstream/master` comes back empty, you're already up to date — skip straight to done.
- Commit or stash any in-progress changes on your working branch before starting step 5, so a merge conflict doesn't get tangled with unrelated edits.
