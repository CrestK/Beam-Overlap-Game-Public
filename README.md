# Find the Overlap

A browser game about the first thing you do every morning in an ultrafast lab: getting the pump
and probe to overlap in space and time.

Each beam is semi-random: new colors, waists, pulse durations, crossing angle, sample lifetime
and starting misalignment. Steer the pump with a kinematic mirror (X and Y tilt) and slide the
delay stage until the pulses meet in the sample. The live cyclohexane spectrum
(200–1700 cm⁻¹) grows as you get closer. Hold at least 95% overlap for a moment to lock in. You
then see how your pulses sat in time against the sample's single-exponential response. Three
beams make a game, and your score is the total time.

Open `index.html` in a browser. There is no build step, so it runs as-is on GitHub Pages or any
static host.

## Levels

| Level | Sample and response | Pulses | Beams (w₀) | Time window for ≥95% |
|---|---|---|---|---|
| Easy | Coumarin 153 in cyclohexane, excited state τ = 4.3 ns | ~150 fs | 85–110 / 45–60 µm | anything after time zero |
| Medium | Cyclohexane Raman bands, τ = 1–2.5 ps | 200–400 fs | 60–75 / 32–42 µm | ~45 µm of stage travel (~300 fs) |
| Hard | Short-lived Raman bands, τ = 0.25–0.6 ps | 50 fs | 35–45 / 18–24 µm | ~8 µm of stage travel (~55 fs) |

The Raman band widths in the spectrum follow the lifetime, Δν = 1/(πcτ), so 1 ps gives 10.6 cm⁻¹.
On easy and medium an amber line marks the signal at full overlap. Hard has no line.

## Controls

- **Mirror**: drag a knob up or right to turn it, scroll over it, or use the arrow keys (Shift for
  bigger steps). The sliders and ± buttons do the same. X moves the pump in the crossing plane,
  Y moves it out of the plane. The small "card" view shows both spots.
- **Delay stage**: drag the carriage for coarse moves. Use the micrometer (0.5 mm per turn) or the
  ± buttons for fine steps. 1 mm of travel is 6.67 ps of delay, because the retroreflector
  doubles the path. The amber bracket is a ruler estimate of where time zero is.
- **Spectrometer**: the bands appear only when the pulses overlap. The trend strip below it
  shows the main peak over the last 20 s, which helps while scanning.

## Scoreboard

After three beams, enter a name to save your total time. The scoreboard keeps the best times
per level in this browser (localStorage), so everyone playing on the same computer can compare.
"Clear scoreboard" on the title screen wipes one level.

## How the overlap is scored

`overlap.js` computes the intensity-weighted overlap of two crossed Gaussian beams with
Gaussian pulses: ∫ D(r, t) · I_probe(r, t) d³r dt. Here D is the sample response driven by the
pump, a population or Raman coherence that rises with the pump and decays as exp(−t/τ). The
model includes diffraction and the pulse-front-tilt smear from the crossing angle. The score
is that integral divided by its maximum over pump position and delay, so 100% is the best
alignment possible for that beam. `game-core.js` generates rounds, maps the mirror and stage to
beam offsets and delay, and models the spectrometer.

## Tests

    npm test
