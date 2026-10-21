# TRIDENT Event Display

An interactive 3D event display for TRIDENT (海铃 Hai-Ling), the proposed deep-sea neutrino telescope in the South China Sea. It runs entirely in a web browser and is built for visualisation and teaching. It is not an official TRIDENT Collaboration product and shows no real TRIDENT data.

- The full array: 1,211 strings in a Penrose-tiling layout with spiral ROV corridors, 24,220 hDOMs, seabed at 3,475 m.
- Event types: νμ CC tracks, νe CC and NC cascades, ντ CC double bangs, the 6.3 PeV Glashow resonance, down-going atmospheric muon bundles, and a custom mode where you aim the neutrino and set its flavour and energy (100 GeV to 100 PeV).
- Cherenkov light in seawater with the measured absorption length, hDOM hits coloured by time, local-coincidence trigger, ⁴⁰K noise, a simple line-fit track reconstruction and a cascade vertex/energy fit.
- Close-up of a single hDOM with its 31 PMTs lighting up.
- Fire, freeze, clear, hideable and resizable panels, export of illustrative data (JSON/CSV).

## Run it

Open the GitHub Pages link of this repository. No installation is needed. `index.html` contains the whole site; it needs an internet connection the first time to load three.js from cdn.jsdelivr.net.

## Keyboard

| Key | Action |
|---|---|
| Space | Fire one event (Manual) or play / pause |
| N | Next event |
| F | Freeze / resume the animation |
| X | Clear event (empty array) |
| R | Reset camera |
| 1–6 | Camera presets (6 = hDOM close-up) |
| C | Cycle view mode (full / modules / hits only) |
| L | Labels on/off |
| H | Hide / show all panels |
| Esc | Clear selection |

## Detector values

From the TRIDENT publications:

- Ye et al. (TRIDENT), *A multi-cubic-kilometre neutrino telescope in the western Pacific Ocean*, Nature Astronomy 7, 1497 (2023): 1,211 strings, 20 hDOMs per string 30 m apart between ~2,800 and 3,400 m depth, Penrose tiling with 70 m and 110 m spacings, ~7.5 km³, site at 3,475 m, absorption length ≈ 27 m, scattering length ≈ 63 m (450–460 nm), ⁴⁰K ≈ 2 kHz per 3-inch PMT.
- Morton-Blake et al., *Optimizing underwater neutrino telescopes for all-flavor point source sensitivity*, Phys. Rev. D 113, 043030 (2026): hDOM with 31 three-inch PMTs and 24 SiPM arrays; trigger of 5 hDOMs, each with ≥2 PMT hits within 20 ns.
- TRIDENT pathfinder PMT system paper and the hDOM design thesis: 17-inch glass sphere, PMT and SiPM areas and efficiencies.

All constants are in `source/js/detector.js` and `source/js/physics.js`, marked `[P]` (from the papers) or `[D]` (display approximation).

## Scientific approximations and limitations

This is an educational display, not TRIDENTSim, Geant4 or real data.

- **Layout**: the Penrose tiling is regenerated (edge 110 m) and trimmed to 1,211 strings, with five spiral corridors; it follows the published pattern but is not the engineering map. The hDOM PMT arrangement is a ring layout of 31 PMTs.
- **Generator**: deep-inelastic kinematics with an approximate inelasticity distribution and lepton angle; no cross sections, so rates and spectra are only what you choose. Glashow resonance with W branching ratios. Atmospheric bundles with an approximate multiplicity, lateral spread and power-law energies.
- **Propagation**: muon energy loss dE/dx = a + bE (a = 0.27 GeV/m, b = 3.4 × 10⁻⁴ /m) with stochastic losses drawn as cascades; τ decay length γcτ (≈ 49 m per PeV) and decay modes; cascades are point-like with a hadronic light-yield factor.
- **Light**: analytic Cherenkov cone for tracks and a peaked angular profile for cascades, attenuated with λ_abs = 27 m; scattering only delays photons statistically; no full photon tracking. ⁴⁰K noise is uncorrelated single hits; bioluminescence and correlated ⁴⁰K coincidences are not modelled.
- **Reconstruction**: least-squares line fit of hit times for tracks; time-residual vertex fit plus total charge for cascades (energy spread about ±45%). Not TRIDENT's own algorithms.
- **Timing**: the animation is slowed by about a million times.
- Exported data are illustrative and not suitable for physics analysis. Downloads are blocked inside some embedded previews; they work on the GitHub Pages site.

## Repository layout

`index.html` at the top is the live site and contains all the code. The `source/` folder has the same code as readable files; after editing them, rebuild `index.html` with `python3 build.py && python3 build_single.py` (esbuild required).

```
source/index.html   page layout and styles
source/js/app.js     interface, animation, rendering loop
source/js/scene.js   three.js array model, hDOM close-up, event visuals
source/js/physics.js generators, propagation, light response, reconstruction
source/js/detector.js geometry and constants (string positions embedded)
source/js/plots.js   array map, hit times, side view, charge vs distance
source/js/worker.js  background thread for the light response
```
