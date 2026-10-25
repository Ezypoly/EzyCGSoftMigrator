# Ezy CG Soft Migrator

Portable Windows tool for backing up and moving settings, presets, scripts,
plug-ins, and other user data between CG software installations.

> [!WARNING]
> Experimental software. Keep an independent copy of important data and close
> affected applications before saving, restoring, removing, or rolling back.

[Download the latest release](https://github.com/Ezypoly/EzyCGSoftMigrator/releases/latest)

## Use

1. Close the applications you want to migrate.
2. Open **Save**, scan, select the required rows, and create a saved copy.
3. On the destination PC, open that folder from **Open / restore**.
4. Review the editable target paths, run **Preview**, and restore.
5. If needed, undo the restore from **Rollback**.

Saved copies are ordinary folders containing `manifest.json`, payload files,
and SHA-256 checksums. Restore merges files and creates a rollback package
before overwriting anything. **Remove selected** also creates a recovery copy
before deletion.

## Scope

The scanner supports 44 Windows creative applications, including Adobe tools,
Blender, Maya, 3ds Max, Cinema 4D, Houdini, ZBrush, Unreal Engine, Unity, Godot,
Krita, GIMP, Affinity, and many others. See [SUPPORTED_APPS.md](SUPPORTED_APPS.md)
for the compact catalog.

The tool migrates files and current-user registry settings; it does not convert
settings between software versions. Native plug-ins and binary preferences may
be version-specific. Licensing, credentials, caches, autosaves, renders, and
project files are excluded where their locations are known.

If something goes wrong, see [RECOVERY.md](RECOVERY.md).

## Build

Requires the .NET 8 SDK:

```powershell
dotnet build -c Release
dotnet publish -c Release -r win-x64 --self-contained true -p:PublishSingleFile=true
```

The release workflow produces `EzyCGSoftMigrator-win-x64.zip`.
