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

## RetroArch CRT shader for 224p and 480i/p consoles

**Base : ``crt-guest-advanced-fast``**
- Good robustness to non integer scaling
- Options enough
- Not too fancy

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
