# Engine Dossier — Prey (CryEngine (Arkane's fork))

> One consolidated, living reference for this game's engine, filled in as the
> `PLAYBOOK.md` phases are worked. Chronological blow-by-blow belongs in the
> `dev-archive/` and `modding-notes/` folders; this file is the *distilled current
> truth*. Update it whenever a fact changes; correct false leads in place.

**Status:** M0, first static look (2026-09-15); the game has not been launched yet. · **VR-readiness verdict:** TBD.

## 1. Identity
- Game / build / version: Prey (2017), Steam build (app 480490). `Binaries\Danielle\x64\Release\Prey.exe` is a 0.5 MB launcher; the game is `PreyDll.dll` (37.9 MB, linked 2019-07-03). `Whiplash\` holds the Mooncrash expansion.
- Platform & store; unofficial port? (extra fragility/legal notes): Steam (PC). Official release, not a fan port.
- Legitimacy: owned copy confirmed.

## 2. Engine lineage
- Family / base engine and how it was modified: **CryEngine**, Arkane's own fork — the build path inside is `D:\_perforce\danielle\cryengineMS\` ("Danielle" is the internal code name) `[inferred-static 2026-09-15]`. Wwise audio, Scaleform UI, Bink 2 video, AMD AGS shipped beside it `[inferred-static 2026-09-15]`.
- Middleware (animation, audio, physics, megatexture, CUDA, etc.):
- Distinctive file formats / build tags / symbol naming: ⭐ **CryEngine's leftover headset controls are still in the build**: console variables `hmd_rotatepitch`, `hmd_rotateroll`, `hmd_rotateyaw`, plus `r_VolumetricCloudsStereoReprojection` and `sys_flash_stereo_maxparallax` (stereo 3D for menus) `[inferred-static 2026-09-15]`. CryEngine shipped stereo rendering in its mainline versions; whether Arkane's fork kept any of it working is unknown `[hypothesis]`.

## 3. Binary & memory
- 32/64-bit, size, module base, ASLR behaviour (stable base? relocations?): **64-bit** (PE32+). `PreyDll.dll` image base `0x180000000`, ASLR on, normal-looking sections (`.text` 28 MB, entropy 6.5) — **no Steam DRM wrapper and no Denuvo string** `[inferred-static 2026-09-15]`.
- Renderer API (D3D11/12, DXGI, GL, Vulkan) with evidence: Direct3D 11 most likely: `d3dcompiler_47.dll` is imported and `d3d11`/`dxgi` names are in the strings, but `d3d11.dll` is **not** a static import, so the renderer is loaded at run time `[inferred-static 2026-09-15]`. `d3d12` names also appear; which one the game actually uses is unchecked.
- Developer console / cvar system present? how opened?: not yet investigated.

## 4. DRM / anti-debug & injection foothold
- DRM (CEG/Denuvo/GOG/none); launch-time-debugger behaviour: Steam API only; no wrapper, no Denuvo string found `[inferred-static 2026-09-15]`. Not tested live.
- Attach workflow that works: not yet tested.
- Injection vector that works (proxy DLL name / injector / framework): not yet tested.

## 5. Threading & frame structure
- Immediate context only, or deferred contexts + command lists?:
- Which thread(s) do what; render-thread name(s):
- One-frame walkthrough (record → replay → present):

## 6. Camera & projection delivery (the crucial section)
- How the world transform reaches the GPU (shared VP buffer / per-draw MVP /
  other), with **shader-reflection / disassembly evidence**:
- Exact constant-buffer slot, parameter name(s), byte offset(s), layout,
  handedness, row/column convention:
- Where projection `P` / FOV comes from:
- The per-eye override maths (`K_eye = …`):

## 7. Constant-buffer fill mechanism
- Map/DISCARD ring / UpdateSubresource / D3D11.1 offset / **persistent map +
  memcpy** (trap):
- Can source contents be read cheaply (captured CPU pointer) or need staging
  read-back?:
- The chosen override patch point and why:

## 8. Pass inventory (by render target)
- Main scene (res/formats):
- Shadow passes (depth-only sizes):
- Post / AA chain (SMAA/TAA/motion vectors; downscale sizes):
- UI / HUD (how it's kept separate):

## 9. cvar / console cheat sheet
| command / cvar | effect | use |
|---|---|---|
| | | |

## 10. Autonomous harness recipe (this game)
- Launch to a known scene (commands used):
- In-process input / camera drive method that worked:
- Frame-capture method; where images land:

## 11. Dead ends & false leads (save future time)
- none yet.

## 12. Open risks toward the North Star
- Nothing blocking seen yet. An unprotected CryEngine build with a console and stereo-related variables still in it is a promising start.
- ⚠️ Stereo variable *names* in a build prove nothing about working code behind them — the Hard Reset project found exactly that trap `[hypothesis]`.
