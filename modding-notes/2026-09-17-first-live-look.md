# 2026-09-17 — first live look: Prey (2017)

Home PC `RTX`, `/lm` session. The user asked for a first look at six games: does each run, and does it still run with our own file added.

## Does it run?

Launches from Steam; `Press Any Key` title, then the main menu (CONTINUE / LOAD GAME / NEW GAME / NEW GAME+ / OPTIONS / LAUNCH PREY: MOONCRASH / EXIT) `[verified-live 2026-09-17, n=4]`. ⚠️ **The user has a 31 h campaign save; NEW GAME is two rows under CONTINUE.**

## With our file added

A 64-bit `dxgi.dll` proxy in `Binaries\Danielle\x64\Release\` now loads and the game reaches the title in a window `[verified-live 2026-09-17, n=1]`. The first build **crashed Prey at start-up** (jump to address 0): x64dbg showed Windows' app-compat shim `AcGenral.dll` calling dxgi's `SetAppCompatStringPointer` before our `DllMain` ran, with an empty address table `[verified-live 2026-09-17, n=1]`; the next build **hung**, because loading the real dxgi calls straight back into that export while our code held a lock `[verified-live 2026-09-17, n=1]`. The generator now answers an export that arrives too early with 0 and retries the real dll with no lock held; the log then shows the real dxgi resolved 20/20 and `CreateDXGIFactory1` + `CompatValue` called. The proxy comes from the shared generator `staging/_shared/proxy-gen/` (every export of the real system dll re-exported with the same ordinals; first call of each export logged). 

## Windowed mode

Append `r_Fullscreen=0`, `r_Width=1280`, `r_Height=720` to `<game>\system.cfg` with the game closed → 1280×720 client window `[verified-live 2026-09-17, n=3]`. Backup: `system.cfg.bak-2026-09-17`.

## How it was driven

Space passes the title. Main menu and options: arrows + Enter; **E** = tab right and opens `Apply changes?`. `WM_CLOSE` exits cleanly with no prompt.

## Dead ends

The in-game video-settings route could not be finished unattended: after Apply, the `Confirm Video Settings` countdown (~5 s) reverted three times — Enter, E and a mouse click all failed to confirm it `[verified-live 2026-09-17, n=3]`. Mouse clicks on main-menu items did not register `[verified-live 2026-09-17, n=2]`.

## Not established

- Nothing past the menus: no gameplay was loaded, no camera data read.
- Every result is from one machine (`RTX`, 21:9 desktop) on one day.

## Next

- [PD] list every console variable in PreyDll.dll (CryEngine registers them by name), especially any `r_Stereo*`/`hmd_*`, and fill in dossier §2–4 and §9
- [FLAT] open the console live (windowed via `system.cfg`) and see whether the stereo/`hmd_` variables exist — without touching NEW GAME
