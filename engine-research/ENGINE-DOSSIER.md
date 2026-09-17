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

**🎮 2026-09-17 (home PC `RTX`, `/lm`) — FIRST LIVE LOOK.**
- **Runs:** Launches from Steam; `Press Any Key` title, then the main menu (CONTINUE / LOAD GAME / NEW GAME / NEW GAME+ / OPTIONS / LAUNCH PREY: MOONCRASH / EXIT) `[verified-live 2026-09-17, n=4]`. ⚠️ **The user has a 31 h campaign save; NEW GAME is two rows under CONTINUE.**
- **With our file added:** A 64-bit `dxgi.dll` proxy in `Binaries\Danielle\x64\Release\` now loads and the game reaches the title in a window `[verified-live 2026-09-17, n=1]`. The first build **crashed Prey at start-up** (jump to address 0): x64dbg showed Windows' app-compat shim `AcGenral.dll` calling dxgi's `SetAppCompatStringPointer` before our `DllMain` ran, with an empty address table `[verified-live 2026-09-17, n=1]`; the next build **hung**, because loading the real dxgi calls straight back into that export while our code held a lock `[verified-live 2026-09-17, n=1]`. The generator now answers an export that arrives too early with 0 and retries the real dll with no lock held; the log then shows the real dxgi resolved 20/20 and `CreateDXGIFactory1` + `CompatValue` called. The proxy comes from the shared generator `staging/_shared/proxy-gen/` (every export of the real system dll re-exported with the same ordinals; first call of each export logged). 
- **Windowed (for measuring; 1280×720 keeps aspect-keyed numbers the same on both PCs):** Append `r_Fullscreen=0`, `r_Width=1280`, `r_Height=720` to `<game>\system.cfg` with the game closed → 1280×720 client window `[verified-live 2026-09-17, n=3]`. Backup: `system.cfg.bak-2026-09-17`.
- **Driving it:** Space passes the title. Main menu and options: arrows + Enter; **E** = tab right and opens `Apply changes?`. `WM_CLOSE` exits cleanly with no prompt.
- **Dead ends:** The in-game video-settings route could not be finished unattended: after Apply, the `Confirm Video Settings` countdown (~5 s) reverted three times — Enter, E and a mouse click all failed to confirm it `[verified-live 2026-09-17, n=3]`. Mouse clicks on main-menu items did not register `[verified-live 2026-09-17, n=2]`.

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
