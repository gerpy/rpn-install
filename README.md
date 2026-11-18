# Retroid Pocket Nova setup notes

## RetroArch CRT Scaling

To maximize screen real estate, the following configurations rely on a dual approach:

- Horizontal Stretching: Expanding the Pixel Aspect Ratio (PAR) aiming around ~4.5% (and strictly capped below the commonly accepted human perception threshold of 5%) to reduce black bars without noticeable geometric distortion. Fixing viewports in this manner also abstracts away the multi-resolution switching chaos of certain consoles, guaranteeing high display consistency.
- Vertical Overscan Cropping: Surgically cutting unused top and bottom margins to enlarge the image, while strictly ensuring all in-game UI elements and HUDs remain fully visible and comfortable.

| System (Core) | Output (Crop V / H) | Displayed res. | Viewport (L × H) | Stretch |
| --- | --- | --- | --- | --- |
| **Atari 2600** (`Stella`) | 160×210 (5 V / 0 H) | 160 × 200 | **1280 × 1008** | 0,00 % (DAR) |
| **NES** (`Mesen`)* | 240×240 (8 V / 8 H*) | 240 × 224 | **1156 × 1028** | +12,50 % (PAR) |
| **Master System** (`GPGX`)* | 248×192 (0 V / 8 G*) | 248 × 192 | **1280 × 960** | +3,23 % (DAR) |
| **PC Engine** (`Beetle PCE`)* | 256×240 (6 V / 0 H) | 256 × 228 | **1212 × 1010** | +12,50 % (PAR) |
| **Super Nintendo** (`Snes9x`) | 256×224 (4 V / 0 H) | 256 × 216 | **1232 × 996** | +8,23 % (PAR) |
| **Mega Drive** (`GPGX`) | 320×224 (0 V / 0 H) | 320 × 224 | **1280 × 960** | -6,67 % (PAR) |
| **Amiga** (`PUAE`) | 320×256 (0 V / 0 H) | 320 × 256 | **1254 × 960** | +4,50 % (PAR) |
| **Nintendo 64** (`Mupen`) | 320×240 (3 V / 4 H) | 312 × 234 | **1312 × 984** | 0,00 % (DAR) |
| **PS1 / Saturn** (`Swan`/`Yaba`) | 320×240 (0 V / 0 H) | 320 × 240 | **1280 × 960** | 0,00 % (DAR) |
| **Arcade / Neo Geo** (`FBNeo`) | Var. (0 V / 0 H) | Var. | **1280 × 960** | --- |
| **Dreamcast** (`Flycast`) | 640×480 (0 V / 0 H) | 640 × 480 | **1280 × 960** | 0,00 % (DAR) |

**(*)** Core settings :
- NES (Mesen) : Overscan Left = 8px et Overscan Right = 8px (Top = 0, Bottom = 0)
- Master System (Genesis Plus GX) : Hide Master System Side Borders = Left Border Only.
- PC Engine (Beetle PCE Fast) : Initial Scanline = 0 et Last Scanline = 239 (disabling the default core crop)

Many retro CRT systems are non integer scaled and require care with shaders.

## RetroArch LCD Scaling

Retro LCD platforms use integer scaling so as to make LCD shaders very clean.

## RetroArch CRT shaders

### Base ``my-guest.slang`` shader

Based on **``crt-guest-advanced-fast``** :

- Good robustness to non integer scaling
- Options enough
- Not too fancy
- Not very power hungry

| Section | RetroArch Parameter | Preset Key | Value |
| :--- | :--- | :--- | :--- |
| Color | Saturation Adjustment | wp_saturation | 1.10 |
| Color | Contrast Adjustment | contr | 0.05 |
| Filtering | Horizontal sharpness | h_sharp | 3.20 |
| Filtering | Substractive sharpness | s_sharp | 0.80 |
| Brightness | Glow Strength | glow | 0.00 |
| Brightness | Bright Boost Dark Pixels | brightboost | 1.75 |
| Brightness | Bright Boost Bright Pixels | brightboost1 | 1.25 |
| Scanlines | Scanline Beam Shape Center | scanline1 | 4.00 |
| Scanlines | Scanline Beam Shape Edges | scanline2 | 5.00 |
| Scanlines | Scanline Shape Dark Pixels | beam_min | 1.15 |
| Scanlines | Scanline Shape Bright Pixels | beam_max | 1.25 |
| Scanlines | Increased Bright Scanline Beam | beam_size | 0.35 |
| CRT Mask | CRT Mask | shadowMask | 10.00 |
| CRT Mask | Mask Strength (0, 5-14) | maskstr | 0.25 |
| CRT Mask | Mask 5-14 Low Strength | mcut | 0.70 |
| CRT Mask | CRT Mask Boost | maskboost | 1.35 |
| CRT Mask | (Transform to) Shadow Mask | mshift | 1.00 |

```ini
#reference "shaders_slang/crt/crt-guest-advanced-fast.slangp"
wp_saturation = "1.100000"
contr = "0.050000"
h_sharp = "3.200000"
s_sharp = "0.800000"
glow = "0.000000"
brightboost = "1.750000"
brightboost1 = "1.250000"
scanline1 = "4.000000"
scanline2 = "5.000000"
beam_min = "1.150000"
beam_max = "1.250000"
beam_size = "0.350000"
shadowMask = "10.000000"
maskstr = "0.250000"
mcut = "0.700000"
maskboost = "1.350000"
mshift = "1.000000"
```

### 3D consoles ``my-guest-2x240.slang`` shaders

For N64 and PSX consoles, I like to render 2x upscaled and then use the shader to reduce the clinical look and harmonize better 2D and 3D elements.

| Section | Paramètre RetroArch | Clé du fichier | Valeur |
| :--- | :--- | :--- | :--- |
| Interlacing Options | Internal Resolution Y | intres | 1.00 |
| Interlacing Options | Downsampling-X | downsample_levelx | 0.75 |
| Interlacing Options | Downsampling-Y | downsample_levely | 0.20 |

```ini
#reference "my-guest.slangp"
intres = "1.000000"
downsample_levelx = "0.750000"
downsample_levely = "0.200000"
```

### 480p and 480i consoles ``my-easy-2x480.slangp``

For such consoles, scanlines don't make sense. Since the default ``crt-guest-advanced-fast`` settings remove them past 370 lines, the ``my-guest.slang`` is OK. For Dreamcast thought, the VGA compatibility produced very clean images so a simpler shader focussed on the mask for a more organic look proves usefull. Let's use the super light ``crt-easymode`` for this one. The Flycast core doesn't offer any antialiasing option so the core has to be configured to render the game at 2x before the preset downscales before ``crt-easymode`` is applied (in a typical SSAA antialiasing way).

| Parameter | `.slangp` Identifier | Value | Role in Pipeline |
| :--- | :--- | :--- | :--- |
| **Pass 0 (SSAA Downscale)** | `shader0` (`stock.slang`) | `0.50` (`Linear`) | Exact 2×2 box average (1280×960 -> 640×480) to smooth 3D edges |
| **Pass 1 (CRT & Mask)** | `shader1` (`crt-easymode.slang`) | `Viewport` (`Nearest`) | 2×2 Lanczos upscale to 1280×960 and phosphor mask application |
| **Sharpness Horizontal** | `SHARPNESS_H` | `0.55` | Optimal balance between 3D blending and 2D sprite/HUD clarity |
| **Sharpness Vertical** | `SHARPNESS_V` | `0.55` | Smooths low-angle stair-stepping (windshield, grille) without blurring text |
| **Mask Strength** | `MASK_STRENGTH` | `0.45` | Diffuse organic grain at 356 PPI without excessive brightness loss |
| **Mask Dot Width** | `MASK_DOT_WIDTH` | `1.00` | Fine 3 px RGB triad tailored to the 1280×960 panel |
| **Mask Dot Height** | `MASK_DOT_HEIGHT` | `2.00` | Vertically extends the pattern into a micro-brick structure |
| **Mask Stagger** | `MASK_STAGGER` | `1.00` | Shifts every other row by 1 px (eliminates continuous vertical stripes) |
| **Mask Size** | `MASK_SIZE` | `1.00` | Native 1:1 scale on the 960p grid |
| **Scanline Strength** | `SCANLINE_STRENGTH` | `0.00` | Scanlines disabled for 480p (2 px per line ratio is insufficient) |
| **Gamma Input / Output** | `GAMMA_INPUT` / `GAMMA_OUTPUT` | `2.30` / `2.20` | Boosts color density (+0.10) without crushing shadow detail |
| **Brightness Boost** | `BRIGHT_BOOST` | `1.15` | Compensates for mask attenuation to match the `my-guest` target |
| **Dilation** | `DILATION` | `1.00` | Approximate linear-light filtering, preserves fine bright details |



```ini
shaders = "2"

shader0 = "shaders_slang/stock.slang"
filter_linear0 = "true"
scale_type0 = "source"
scale0 = "0.500000"

shader1 = "shaders_slang/crt/shaders/crt-easymode.slang"
filter_linear1 = "false"
scale_type1 = "viewport"

SHARPNESS_H = "0.550000"
SHARPNESS_V = "0.550000"
MASK_STRENGTH = "0.450000"
MASK_DOT_WIDTH = "1.000000"
MASK_DOT_HEIGHT = "2.000000"
MASK_STAGGER = "1.000000"
MASK_SIZE = "1.000000"
SCANLINE_STRENGTH = "0.000000"
GAMMA_INPUT = "2.300000"
GAMMA_OUTPUT = "2.200000"
BRIGHT_BOOST = "1.150000"
DILATION = "1.000000"
```

If the core offers good antialiasing options such as MSAA, then the first pass is no longer required and the shader preset is :

