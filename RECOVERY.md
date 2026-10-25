# Recovery

## Undo a restore

1. Close the affected application.
2. Open **Rollback** in Ezy CG Soft Migrator.
3. Select the entry matching the restore time.
4. Choose **Revert selected restore** and review the result.

Rollback packages are stored in:

```text
Documents\EzyCGSoftMigrator Rollbacks\<timestamp>
```

Existing files and registry values are restored from the pre-restore snapshot.
Files created by the restore are removed only when their SHA-256 checksum still
matches the restored file. A changed file is skipped to protect newer work.

## Recover removed settings

Recovery copies created by **Remove selected** are stored in:

```text
Documents\EzyCGSoftMigrator Removed Settings\EzyCGSoftBackup_<timestamp>
```

Open the folder containing `manifest.json` from **Open / restore**, check the
target paths, run **Preview**, and restore only the required rows.

## If automatic rollback fails

Do not restore over the same destination again. Copy the rollback folder and
the current application profile somewhere safe, then inspect
`rollback-manifest.json`. For registry entries, use the application's automatic
rollback; `registry-before.json` is an internal snapshot, not a `.reg` file.

If the application no longer starts, rename its current profile while the
application is closed, let it create a clean profile, and restore only known-good
categories.
