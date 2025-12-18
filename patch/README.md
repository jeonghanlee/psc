# Applying Patch

This document outlines the workflow to apply the `qspi.patch` file (located in the `patch/` folder) to the source directory within the `lbl-psc` branch.

## Prerequisites

1.  Ensure you are on the target branch:
```bash
git checkout lbl-psc
```
2.  Ensure the patch file exists at `patch/qspi.patch`.

## Step-by-Step Workflow

### 1. Dry Run (Safety Check)
**Always execute this step first.** It checks for conflicts without modifying any files.
* `-p0`: Required because the patch was created with `--no-prefix` (Level 0).

```bash
git apply -p0 --check patch/qspi.patch
```

* **No Output:** Success. The patch is clean. Proceed to **Step 2**.
* **"patch failed" Error:** Conflicts detected. Go to the [Troubleshooting](#-troubleshooting) section.

### 2. Apply the Patch
If the dry run was successful, apply the patch permanently.
* `--whitespace=fix`: Automatically corrects trailing whitespace errors.

```bash
git apply -p0 --whitespace=fix patch/qspi.patch
```

### 3. Verify
Review the applied changes to ensure correctness.

```bash
# Check modified files
git status

# Review code changes
git diff
```

---

## Rollback (If needed)
**Only execute this if you want to undo the patch application (e.g., if you made a mistake).**

```bash
git restore .
```

---

## 🔧 Troubleshooting

### Resolving Conflicts (`.rej` files)
If **Step 1** fails, the codes have diverged. Follow these steps to resolve conflicts manually:

1.  **Force Apply with Rejects:**
    This applies the successful parts and saves failed chunks to `.rej` files.
    * Note: `-p0` is still required here.
    ```bash
    git apply -p0 --reject patch/qspi.patch
    ```

2.  **Manual Resolution:**
    * Navigate to the `psc/` directory.
    * Look for files ending in `.rej` (e.g., `main.c.rej`).
    * Open the corresponding source file and manually add the code shown in the `.rej` file.
    * **Delete** the `.rej` files once you have fixed the code.

3.  **Finalize:**
    Once all conflicts are resolved and `.rej` files are deleted, proceed to commit.
