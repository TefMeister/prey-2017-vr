# Prey VR prior art: fholger is building a Prey VR mod, and Chairloader already gives a console and a CVar dump

**Status:** 🆕 new · **Priority:** high — it answers both open board rows and names the most
relevant VR modder for this exact game.

## What is public

- **fholger is working on a Prey (2017) VR mod.** fholger is the author of the open-source *Crysis VR*
  and *Far Cry VR* mods (both CryEngine, OpenXR, 6DoF and motion controls). A Patreon post titled
  "Prey (2017) VR Development Update" reports that basic stereo worked quickly years ago, that roomscale
  movement and motion controls were the roadblocks, and that work has resumed and got past several of
  them `[reported 2026-09-17, search snippet; the post itself returned HTTP 403 to automated fetch, so
  open it in a browser]`.
- **Chairloader** by **thelivingdiamond** is the Prey modding framework (Nexus mod 103, open source on
  GitHub). It loads mod DLLs at runtime and re-implements a **full in-game console** plus a free camera,
  entity editor and spawner `[reported]`. FRAMED's Prey guide: press **F1** for the mod menu, then
  *Chairloader → Show Console* `[reported]`.
- Chairloader's console menu has **"Dump Commands to the console"** and **"Dump CVars to file"**
  `[reported]` — a ready-made answer to the board's "list every console variable" row, taken from the
  running game instead of from strings in `PreyDll.dll`.
- FRAMED lists `ed_ViewFov` and `ed_ViewMaxSpeed` / `ed_ViewMinSpeed` as useful CVars `[reported]`.
- CryEngine's public documentation still describes `r_StereoOutput` (stereo output modes, including a
  headset mode) `[reported]`; the dossier already found `hmd_rotatepitch/roll/yaw` in the build.
- fholger's **Crysis VR** source is public (<https://github.com/fholger/crysis_vrmod>, OpenXR,
  32- and 64-bit) — study material for how a CryEngine game was given two eye views and motion
  controls. Its licence is not a standard SPDX one; read it before quoting anything, and copy nothing.

## Why it matters here

1. **The CVar list can come from Chairloader's dump**, cross-checked against the static strings. If
   `r_Stereo*` / `hmd_*` survive as live CVars, their current values are in that file.
2. **The console-open `[FLAT]` row may not need our own work at all**: Chairloader's console is a
   known, public route.
3. **fholger's project is the prior art to watch.** If it ships, our value is in what it does not do.
   Either way, his CryEngine write-ups are the best public explanation of the technique.

## Next step

Install Chairloader on a copy, dump CVars to file, and compare against the dossier's static list.
Read fholger's Patreon post in a browser.

## Sources

- Chairloader — <https://www.nexusmods.com/prey2017/mods/103>, <https://github.com/thelivingdiamond/Chairloader>, <https://chairloader.dev/>
- FRAMED Screenshot Community, Prey guide — <https://framedsc.com/GameGuides/prey.htm>
- fholger, "Prey (2017) VR Development Update" — <https://www.patreon.com/fholger/posts/prey-2017-vr-165375021>
- fholger, Crysis VR — <https://github.com/fholger/crysis_vrmod>
- CryEngine documentation, RSTEREOOUTPUT — <https://docs.cryengine.com/display/CRYAUTOGEN/RSTEREOOUTPUT>
