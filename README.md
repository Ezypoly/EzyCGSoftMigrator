# Ezy CG Soft Migrator

Portable Windows tool for backing up and moving settings, presets, scripts,
plug-ins, and other user data between CG software installations.

> [!WARNING]
> Experimental software. Keep an independent copy of important data and close
> affected applications before saving, restoring, removing, or rolling back.

[Download the latest release](https://github.com/Ezypoly/EzyCGSoftMigrator/releases/latest)

## How it works

The application scans known Windows locations and shows only settings found on
the current PC. Each row represents one settings set, such as Blender's user
profile, Maya preferences, or Unity editor preferences.

- **Save** creates a portable folder containing `manifest.json`, copied data,
  and SHA-256 checksums.
- **Update saved copy** refreshes selected sets inside an existing saved copy.
- **Open / restore** maps saved sets to detected application versions. Every
  destination remains editable.
- **Preview** reports how many files will be copied or replaced before restore.
- **Rollback** undoes a restore using the recovery package created beforehand.
- **Remove selected** makes a recovery copy and then removes the selected local
  settings.

Restore merges files into the destination. It does not delete unrelated files.

## Save and restore

1. Close the applications whose settings you want to move.
2. Open **Save** and choose **Scan**.
3. Review the paths and select the required rows. Large or cache-containing
   locations are not selected automatically.
4. Choose a library folder and create the saved copy.
5. Move that saved-copy folder to the destination PC if necessary.
6. Open **Open / restore** and select the folder containing `manifest.json`.
7. Review each proposed destination. Change a path if the detected version is
   not the one you want.
8. Choose **Preview**, read its warnings, and then restore.
9. Launch the target application and verify the result before deleting the
   saved copy or rollback package.

## Moving to another software version

Ezy CG Soft Migrator proposes the newest detected destination for the same
application and category. This is file and registry migration, not settings
format conversion. Text configuration, presets, scripts, and brushes are often
portable; native plug-ins and opaque binary preferences may be version-specific.

For a safer cross-version migration, launch the new application once, close it,
restore only the categories you need, and keep the rollback package until you
have tested the result.

## Safety and recovery

- Every payload file is checked against its SHA-256 value before restore.
- Existing destination files and registry values are captured before overwrite.
- A rollback manifest is updated during restore, so a partially completed
  operation can usually be reverted.
- Rollback removes a newly created file only if it still matches the file that
  restore wrote. Files changed afterward are preserved and reported as skipped.
- Known running applications block restore and removal.
- Directory links are not followed while creating saved copies.

If a restore or removal went wrong, close the affected application and follow
[RECOVERY.md](RECOVERY.md) before making further changes.

## Scope and limits

The scanner supports 44 Windows creative applications, including Adobe tools,
Blender, Maya, 3ds Max, Cinema 4D, Houdini, ZBrush, Unreal Engine, Unity, Godot,
Krita, GIMP, Affinity, and others. See [SUPPORTED_APPS.md](SUPPORTED_APPS.md).

Licensing, credentials, caches, autosaves, renders, and project files are
excluded where their locations are known. Locations under Program Files,
ProgramData, or Common Files may require administrator rights.

## Build

Requires the .NET 8 SDK:

```powershell
dotnet build -c Release
dotnet publish -c Release -r win-x64 --self-contained true -p:PublishSingleFile=true
```

The release workflow produces `EzyCGSoftMigrator-win-x64.zip`.
