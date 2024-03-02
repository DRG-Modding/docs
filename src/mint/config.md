# Configuration, Mod Data, Cache and Log File

Mint stores the user's configuration, mod data (profiles and its mods) and cache under these
directories on Windows:

- Config and log directory: `C:\Users\<username>\AppData\Roaming\mint\config`.
  Noteable files:
    - Configuration file: `config.json`. **Do not** share this file with another person unless you
      removed the mod.io OAuth token from your mod.io account.
    - Log file: `mint.log`. Please try to include the log file when trying to report
      bugs as it helps us to diagnose the issue.
- Cache directory: `C:\Users\<username>\AppData\Local\mint\cache`. Noteable file:
    - Cache index: `cache.json`.
- Data directory: `C:\Users\<username>\AppData\Roaming\mint\data`.

## Note for users migrating from older versions of mint

Previously, these directories were:

- `C:\Users\<username>\AppData\Roaming\drg-mod-integration\config`
- `C:\Users\<username>\AppData\Local\drg-mod-integration\cache`
- `C:\Users\<username>\AppData\Roaming\drg-mod-integration\data`
