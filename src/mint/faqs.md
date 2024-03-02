# Frequently Asked Questions (FAQs)

## Why Does mod XXX not work?

See Mod Compatibility Issues under Common Errors and Troubleshooting.

## What are Mod Lints?

Mod Lints are a set of tools designed to help find potential problems in the mods of the current
profile. These problems can include conflicting files, mods containing unmodified game assets, etc.
Mod Lints can be especially helpful when developing, packaging and cooking mods locally.

## Can Mint be displayed in other languages?

Right now, unfortunately we don't have a good way to do localization. If we could, Mint would likely
come with Simplified Chinese as well as potentially other community-contributed localizations.

## Is there an active mod limit?

Unlike the official mod integration, Mint does not impose mod limits.

## Why does it say modding disabled due to launch options

Mint disables the official mod integration system when you click Install Mods. To restore the
official mod integration system, you will need to click Uninstall Mods.

## Will mod XXX work for clients?

This depends on the particular mod. For `RequiredByAll` mods, it is strongly recommended that every
member of the lobby has the mod installed to prevent desync. For example, clients need to install
Custom Difficulty if the resupply nitra cost is modified or else they cannot call a cheaper resupply
without the default 80 nitra in the team repository.

## Does Mint disable achievements?

Mint does not attempt to disable achievements, regardless of the approval categories of the mods
being used.

## Does Mint force a sandbox save?

Mint does not attempt to force a sandbox save. If you use gameplay affecting mods, it is your
judgement on if it is acceptable to affect the normal saves. It is also recommended to make a
backup of your normal saves (even without Mint, this is a good idea).

## What's the black console showing up when mint launched?

This console window is used for logging and debugging purposes, to help us diagnose errors that you
run into while using Mint.

## Where do I add mods?

See the Adding Mods section under Getting Started.

## Why does Mint add `[MODDED]` tag to my lobby name?

We do not want clients to unknowingly join lobbies that have gameplay affecting mods. If your
profile contain any non-verified mods, your lobby name will have `[MODDED]` prepended to it to
inform potential players who might want to join your lobby.

## Why is mod XXX still active after I disable it?

You need to click Install Mods after making changes to the current profile for changes to take
effect.
