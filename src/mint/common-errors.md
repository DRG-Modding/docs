# Common Errors and Troubleshooting

## Mod Compatibility Issues

Some mods may work under official mod integration but not in mint. This can be because:

- Official mod integration does not load all the files in a mod, but Mint does. Additionally
  loaded files may cause issues in game.
    - May require the mod to be updated to remove extra files.
- The mod interacts with `AssetRegister.bin`. The official mod integration partially handles
  `AssetRegister.bin` merging, but Mint currently does not implement it.

## Failure While Updating Cache (403, 404)

You may see the following message

```
ERROR mint::gui::message: modio::Error {
    kind: Status(
        403,
    ),
    source: Error {
        code: 403,
        error_ref: 15024,
        message: "The mod ID you have included in your request could not be found.",
        errors: None,
    },
}
```

when trying to update your cache. One of your mod.io mods may be renamed or deleted by its author.
This is currently a bug in Mint that the cache update might get stuck on 403/404 if there is a mod
which got renamed or deleted. The workaround is to:

- Remove the renamed/deleted mod.io mod.
- Remove the cache directory: delete `C:\Users\<username>\AppData\Local\mint\cache`.
- Press Install Mods again.

## Install mods failed with "An existing connection was forcibly closed by the remote host" (OS error 10054)

This can be due to strange mod.io rate limit or network issues. Try waiting for a few minutes and
then try installing mods again.

## ureq error: discord attachment/oo2core_9_win64.dll: status code 403

Please see <https://github.com/trumank/mint/issues/134>.
