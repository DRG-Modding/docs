# Mod Load Order and Conflict Resolution

Mint's mod load order is *bottom first top last*. This means that a mod will overwrite any
conflicting files that mods below it might also modify.

## Example

If two mods A and B both modify the same files, and they are ordered inside Mint like

- A
- B

Then A will win and overwrite files previously modified by B.
