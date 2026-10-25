# Recovery and rollback

Use this guide after a wrong restore, an interrupted restore, or removal through
**Remove selected**.

## First steps

1. Close the affected application. Applications often rewrite preferences when
   they exit, so do not keep reopening it while investigating.
2. Keep the original saved copy unchanged.
3. Keep the matching recovery folder unchanged. If the data is important, copy
   both folders to another drive before continuing.

Restore rollback packages are stored in:

```text
Documents\EzyCGSoftMigrator Rollbacks\<timestamp>
```

Recovery copies made before removal are stored in:

```text
Documents\EzyCGSoftMigrator Removed Settings\EzyCGSoftBackup_<timestamp>
```

## Undo a restore

1. Open **Rollback** in Ezy CG Soft Migrator.
2. Choose **Refresh** if the expected entry is missing.
3. Select the entry whose time matches the problematic restore.
4. Choose **Revert selected restore** and confirm the listed destination.
5. Review the result before opening the graphics application.

The result counters mean:

- **Restored files** — earlier files were copied back.
- **Removed files created by restore** — files that did not exist before the
  restore were removed.
- **Restored registry sets** — current-user registry settings were returned to
  their previous snapshot.
- **Skipped files changed since restore** — a file no longer matches what the
  restore wrote, so rollback preserved the newer version.

Rollback affects only items recorded for that restore. It does not delete other
files from the application profile.

## If files were skipped

A skipped file was changed after restore, usually because the application was
opened or the user edited it. Automatic rollback intentionally keeps that file.

1. Make a separate copy of the current file.
2. Select the rollback entry and choose **Open folder**.
3. Open `rollback-manifest.json` and find the matching `RelativePath`.
4. Compare the current file with the copy inside the rollback package.
5. Replace it manually only after deciding which version should be kept.

Do not copy the entire rollback folder over an application profile. It contains
manifests and internal entry directories, not a directly reusable profile tree.

## Interrupted restore

Open **Rollback** and inspect the newest timestamped entry. The rollback
manifest is saved before each destination file is changed, so the completed
portion can normally be reverted even when the restore did not finish.

If no usable entry appears, preserve the rollback folder and current profile
before attempting anything manually.

## Recover settings removed from the PC

**Remove selected** creates a normal saved copy before deleting settings:

1. Open **Open / restore**.
2. Select the recovery folder containing `manifest.json`.
3. Check every proposed destination path.
4. Run **Preview** and restore only the required rows.

This restore creates its own rollback package and can be reverted normally.

## If the application no longer starts

Try automatic rollback first. If that is not possible:

1. Close the application and copy its current profile somewhere safe.
2. Rename the broken profile folder.
3. Start the application once so it creates a clean profile, then close it.
4. Restore only known-good categories or individual files.

Do not remove or rename a profile while its application is running.

## Manual recovery

Use manual recovery only when automatic rollback cannot run. Each entry in
`rollback-manifest.json` records its `TargetPath`, `Kind`, `BackupDirectory`,
and affected `Files`. An item with `ExistedBefore: true` has a previous copy in
the rollback package. An item with `ExistedBefore: false` was created by restore.

Delete a newly created file manually only when its current SHA-256 still equals
`AppliedSha256`; otherwise keep it for inspection. You can calculate the value
with:

```powershell
Get-FileHash -Algorithm SHA256 -LiteralPath "C:\exact\path\to\file"
```

Do not import `registry-before.json` into Registry Editor. It is an internal JSON
snapshot, not a `.reg` file. Preserve it and use automatic rollback whenever
registry recovery is required.
