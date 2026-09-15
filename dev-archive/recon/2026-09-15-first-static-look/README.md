# First static look (2026-09-15)

Read from the installed Steam copy on the home PC (`C:\Steam\steamapps\common\Prey`), without launching the game.
Every claim below is `[inferred-static 2026-09-15]` unless tagged otherwise: it comes from reading file
headers and strings, not from running anything.

- **Install:** 31 GB.
- **Identity:** Prey (2017), Steam build (app 480490). `Binaries\Danielle\x64\Release\Prey.exe` is a 0.5 MB launcher; the game is `PreyDll.dll` (37.9 MB, linked 2019-07-03). `Whiplash\` holds the Mooncrash expansion.
- **Engine:** **CryEngine**, Arkane's own fork — the build path inside is `D:\_perforce\danielle\cryengineMS\` ("Danielle" is the internal code name) `[inferred-static 2026-09-15]`. Wwise audio, Scaleform UI, Bink 2 video, AMD AGS shipped beside it `[inferred-static 2026-09-15]`.
- **Binary:** **64-bit** (PE32+). `PreyDll.dll` image base `0x180000000`, ASLR on, normal-looking sections (`.text` 28 MB, entropy 6.5) — **no Steam DRM wrapper and no Denuvo string** `[inferred-static 2026-09-15]`.
- **Renderer:** Direct3D 11 most likely: `d3dcompiler_47.dll` is imported and `d3d11`/`dxgi` names are in the strings, but `d3d11.dll` is **not** a static import, so the renderer is loaded at run time `[inferred-static 2026-09-15]`. `d3d12` names also appear; which one the game actually uses is unchecked.
- **Protection:** Steam API only; no wrapper, no Denuvo string found `[inferred-static 2026-09-15]`. Not tested live.
- **Other:** ⭐ **CryEngine's leftover headset controls are still in the build**: console variables `hmd_rotatepitch`, `hmd_rotateroll`, `hmd_rotateyaw`, plus `r_VolumetricCloudsStereoReprojection` and `sys_flash_stereo_maxparallax` (stereo 3D for menus) `[inferred-static 2026-09-15]`. CryEngine shipped stereo rendering in its mainline versions; whether Arkane's fork kept any of it working is unknown `[hypothesis]`.

## Method

PE headers and import tables read with `pefile`: machine type, link timestamp, image base, ASLR
flag, section names, sizes and entropy. Then a case-insensitive search of each binary for renderer
DLL names (`d3d9`, `d3d11`, `d3d12`, `dxgi`, `vulkan-1`, `opengl32`), headset runtimes (`openvr`,
`openxr`, `oculus`), protection markers (`denuvo`, `securom`, `.bind`) and middleware names, with
readable strings pulled around the interesting hits. A string match shows a name is present in the
file, not that the code path is used. A `.text` section with entropy near 8.0 is encrypted or
compressed, not normal code.

## Risks noted

- Nothing blocking seen yet. An unprotected CryEngine build with a console and stereo-related variables still in it is a promising start.
- ⚠️ Stereo variable *names* in a build prove nothing about working code behind them — the Hard Reset project found exactly that trap `[hypothesis]`.
