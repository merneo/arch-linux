# Arch Linux Repository - Local Setup

**Location:** `~/arch-linux/`  
**Purpose:** Local copy of public arch-linux repository

---

## Repository Status

This is a local copy of the arch-linux repository containing only wiki documentation for Arch Linux installation.

### Contents

- **Root documentation:** 6 markdown files (e.g., README.md, MODULE_index.md)
- **Phases:** 10 installation phases
- **Steps:** 25 installation steps (concise commands)
- **Modules:** 34 detailed modules (now unique, no longer mirroring steps/)
- **Wiki:** 6 wiki documentation files

**Total:** ~81 markdown files (wiki documentation + various guides)

### Development Files

- ✅ **0 development files** - All moved to `~/helpers/arch-linux/`
- ✅ **No AI helpers** - All moved to private repository
- ✅ **Clean repository** - Ready for public use

---

## Git Configuration

For comprehensive documentation on Git commands, refer to the [Git Reference Manual](https://git-scm.com/docs).

### If Git Repository Exists

```bash
cd ~/arch-linux
git remote -v # Lists current remote repositories. See [Git: git-remote](https://git-scm.com/docs/git-remote).
# Should show: git@github.com:merneo/arch-linux.git
```

### If Git Repository Needs Setup

```bash
cd ~/arch-linux
git init # Initializes a new Git repository. See [Git: git-init](https://git-scm.com/docs/git-init).
git remote add origin git@github.com:merneo/arch-linux.git # Adds a new remote named 'origin'. See [Git: git-remote](https://git-scm.com/docs/git-remote).
git fetch origin # Downloads objects and refs from another repository. See [Git: git-fetch](https://git-scm.com/docs/git-fetch).
git checkout -b main origin/main  # Checks out a branch. See [Git: git-checkout](https://git-scm.com/docs/git-checkout). # or master, depending on remote
```

---

## Related Repositories

### Public Repository
- **Location:** `~/arch-linux/` (this directory)
- **GitHub:** https://github.com/merneo/arch-linux
- **Content:** Wiki documentation only
- **Visibility:** Public

### Private Repository (Helpers)
- **Location:** `~/helpers/arch-linux/`
- **GitHub:** Private repository (to be created)
- **Content:** Development/AI helpers
- **Visibility:** Private

---

## Verification

### Check Repository is Clean

```bash
cd ~/arch-linux
# Should return 0
find . -name "*EDUCATIONAL*" -o -name "*COMMENTED*" -o -name "*CONSISTENCY*" | wc -l
```

### Check File Counts

```bash
cd ~/arch-linux
echo "Root files: $(ls -1 *.md | wc -l)" # Expected: 6
echo "Phases: $(find phases -name '*.md' | wc -l)" # Expected: 10
echo "Steps: $(find steps -name '*.md' | wc -l)" # Expected: 25
echo "Modules: $(find modules -name '*.md' | wc -l)" # Expected: 34
echo "Wiki: $(find wiki -name '*.md' | wc -l)" # Expected: 6
```

---

**Last Updated:** 2025-12-03  
**Status:** ✅ Local repository ready
