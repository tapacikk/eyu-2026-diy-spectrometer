# 3D printed fiber spectrometer

STL and FreeCAD files for the fiber spectrometer case in this directory:

- `Spectrometer.stl` — case base
- `Spectrometer_Lid.stl` — lid
- `Spectrometer_FreeCAD.FCStd` — FreeCAD source model (edit this, then re-export)

Light enters through an optical fiber or a slit, a diffraction grating spreads
it into a spectrum and a small USB webcam reads it. The entrance slit is part
of the printed case — there is no separate slit foil to install.

This design is based on the GaudiLabs
[3D Printable Fiber Spectrometer](https://github.com/GaudiLabs/3DFiberSpectrometer/tree/main)
(GPLv3, Urs Gaudenz), with the case re-dimensioned to fit a mini webcam board
other than the one the original kit ships with. The step-by-step assembly
walkthrough on the GaudiLabs site
(https://www.gaudi.ch/GaudiLabs/?page_id=1124) applies here except where noted
below. Many thanks to the creators.

# WARNING — the models are 10x oversized

The STL and FreeCAD files are modelled 10x larger than the real part. Scale down
by 10x (10 %, factor 0.1) before printing, and remember the same when editing:
in FreeCAD 10 mm is 1 mm in reality. I modelled in centimetre-like units; the
files are not wrong, they are just not to scale. Sorry about that.

As printed the base is 60 mm wide, 11 mm deep and 127 mm long; the lid is
60 x 127 x 5 mm. If your slicer shows a part around 1270 mm long, the scale is
still 1:1 and it will not fit any camera or fiber.

## Bill of materials

| Part | Notes |
|---|---|
| Case + lid print | Files in this directory, printed in black PLA |
| Camera | FTVOGUE Mini USB2.0 Webcam Board, OV7675 sensor. **This is the board the camera pocket is dimensioned for.** A different board needs the FreeCAD model adjusted  |
| Optical fiber cable | Any standard audio (TOSLINK / optical 3.5 mm) cable works. |
| Diffraction grating foil | 1000 lines/mm grating foil. You can also try using a CD/DVD disk |
| White LED | Light source |
| Coloured / RGB LED | Optional, shows that coloured light only contains part of the spectrum |
| 3 V coin cell (CR2032) | To drive the loose LEDs |
| Cuvettes | Only for absorption measurements, 10 mm path length |

## Print settings

- Black matte PLA, 0.4 mm nozzle, 0.16–0.2 mm layers, 100 % infill, heated bed,
  10 % scale, no supports.

Burrs and stringing matter more than on a normal print.  Clean both with a
knife before assembling.

## Calibrating with spectro-cam-rs

[spectro-cam-rs](https://github.com/DerFetzer/spectro-cam-rs) is the app used
with this build (Linux/Windows/macOS). It maps pixel column to wavelength with a
straight line through two calibration points and ships with 436 nm at column 261
and 546 nm at column 486 — the mercury lines of a fluorescent lamp on the camera
the author used. Yours will be different.

What you need is a light source with at least two sharp, known lines.

- **Laser pointer** — easiest, and what this build was calibrated with. A laser
  diode emits one very narrow line, so you need two lasers with known
  wavelengths (common ones: 405 nm violet, 532 nm green, 650 nm red). Bounce
  the light from the pointer into the slit so the line is not so bright that
  the sensor in saturated.
- **Fluorescent lamp / CFL** — gives several mercury lines at once: 405 nm
  (violet, often weak), 436 nm (blue), 546 nm (green), 577 + 579 nm (yellow
  doublet, usually merged into one peak), 611 nm (orange). 

Procedure:

1. Open the Calibration window. (It also helps to keep the camera preview
   window open, as you will see which pixels you're setting) Two vertical lines
   appear in the spectrum plot.
2. [Optional] Set the averaging buffer in the Postprocessing window high enough for the
   spectrum to stop jumping around (a few dozen frames is plenty).
3. Put the first reference line in front of the fiber. Type its wavelength into
   **Low Wavelength** (or **High Wavelength**, whichever end of the sensor it
   falls on) and slide the matching **Low/High Index** slider until the measured
   peak sits exactly on its vertical line. The line is fixed at the wavelength
   you typed; the peak moves as you change the index.
4. Repeat for the second line at the other end of the sensor.
5. Check with a third line: read the nm labels above the peaks. An IR LED
   should land near 950 nm.
6. Optionally improve the shape of the curve: the **Linearize** dropdown applies
   the camera's (Rec601 / Rec709 / sRGB) response curve, and **Gain R/G/B**
   compensates for the sensor's uneven colour sensitivity. Neither changes the
   pixel→nanometre mapping, so calibrate first.

Calibration is stored in the app's config, so you only do this once per camera.

## Absorption measurements with a cuvette

The fiber connection is what makes absorption easy: the fiber goes into a cuvette
holder, light passes through the liquid and the spectrum is compared against a
blank. The holder and cap are CuvetteHolder.stl and CuvetteCap.stl files. Print
them with the same settings as the case.

1. Press the optical fiber into the holder and put the white LED with its coin
   cell into the hole on the far side, so the light goes through the liquid and
   into the fiber.
2. Fill a cuvette with the pure solvent, insert it in the correct orientation
   (the two optically clear windows must be in the light path) and cover it with
   the cap — stray light entering the cuvette shows up as a fake absorption.
3. In spectro-cam-rs, with the blank in place and the reading stable, press
   **Set Current As Zero Reference** in the Calibration window.
4. Swap in the sample. The plot now shows absorption relative to the blank.
   **Clear Zero Reference** returns to raw intensity.

## Credits and license

Original design, instructions and the cuvette holder: Urs Gaudenz / GaudiLabs,
https://github.com/GaudiLabs/3DFiberSpectrometer/tree/main (GPLv3). This case
variant is released under the same license, GPLv3 — see the `LICENSE` file in the
repository root.
