# Hydrokinetic Turbogenerator

A run-of-river turbogenerator — 3D-printed vertical-axis turbines driving a brushless
hub motor repurposed as a three-phase generator. Designed, built and field-tested over
the course of a year as a secondary-school research project.

![Carrying the rig into the river Erft for the field test](docs/photos/01-field-deployment.jpg)

**It did not work.** The turbine would not turn in the river. That result, and what I
now think caused it, is the most useful part of this repository — see
[Why it didn't turn](#why-it-didnt-turn) below.

## Context

Written as a *Projektkurs Naturwissenschaften* thesis (Friedrich-Wilhelm-Gymnasium,
June 2024), before I started my mechanical engineering degree at ETH Zürich. The
question was whether a small, 3D-printable hydrokinetic generator could produce useful
power from a river without a dam — the kind of thing that could be carried to a place
with no grid connection.

The full report is in German: [`docs/report-de.pdf`](docs/report-de.pdf).

## What was built

![The complete rig at the riverbank](docs/photos/02-assembly-riverside.jpg)

Left to right: the 3D-printed turbine housing, the hub-motor generator on its threaded
shaft, the motor mount, and the sealed junction box holding the rectifier. Everything
sits on a concrete paving slab — added late in the build for mass and stiffness, after
the original design proved too light to stay put in flowing water.

![Component layout from above](docs/photos/03-assembly-topdown.jpg)

### Turbines

Ten vertical-axis turbine variants, printed and weighed, varying three parameters
independently:

- **Blade count** — 3, 4, 5 or 6
- **Blade form** — straight or curved
- **Additions** — none, guide vanes, or a second set of reduced-size blades

All variants share the same envelope: **170 mm tall, 86–100 mm diameter**, massing
**90 g to 158 g**. Mass rose consistently with blade count, and the guide-vane variant
was by far the heaviest — roughly 30 % above the plain three-blade turbine, which
loads the generator's bearing correspondingly harder.

![Curved and straight blade variants](docs/photos/09-turbine-variants-cad.jpg)

The housing was designed around a reversible fit so that any turbine could be swapped
in without rebuilding the rig.

### Generator

A hoverboard hub motor, stripped down and used in reverse as a three-phase permanent
magnet generator.

![Hoverboard hub motor teardown](docs/photos/05-hoverboard-teardown.jpg)
![Stator windings and phase leads](docs/photos/06-stator-windings.jpg)

### Rectification

The generator's three-phase AC output is rectified by a six-diode full bridge, built
into a sealed junction box together with the load resistor and measurement taps.
Circuit diagram: [`docs/rectifier-circuit.pdf`](docs/rectifier-circuit.pdf).

![The rectifier bridge in its housing](docs/photos/07-rectifier.jpg)

## Results

**Bench test, turned by hand.** 4.19 V across 3.41 A into a 20 Ω load — about
**14.3 W**. The generator and rectifier chain worked.

**Field test.** River Erft at Bergheim, flow velocity measured at **1.468 m/s** by
timing a leaf over a 10 m stretch. The rig was placed on the bed with the turbine
20 cm below the surface, aligned square to the flow.

![Measuring output at the riverbank](docs/photos/08-field-measurement.jpg)

**The turbine did not rotate.** Spun by hand underwater it produced 1.64 V at 1.21 A
(**≈ 2.0 W**) and coasted for one or two seconds before the generator's drag stopped
it. The assembly stayed watertight, which was the thing I had most expected to fail.

Because the turbine never reached a steady speed under its own power, no power
coefficient could be measured. Assuming a plausible *c*<sub>p</sub> ≈ 0.1, the report
projected 2.28 W.

## Why it didn't turn

*This section is a retrospective added in 2026 — it is not part of the original
report, which stopped at reporting the null result.*

Running the numbers again, the diagnosis is clearer, and it is not the one I assumed
at the time. The kinetic power passing through the swept area
(*A* = 0.01439 m², *v* = 1.468 m/s) is

$$P = \tfrac{1}{2}\rho A v^{3} = \tfrac{1}{2}\cdot 1000 \cdot 0.01439 \cdot 1.468^{3} \approx 22.8\ \mathrm{W}$$

Even capped at the Betz limit (*c*<sub>p</sub> ≤ 0.593), roughly **13 W** was
physically available. There was no shortage of power in the flow.

**So the binding constraint was starting torque, not power.** Two mechanisms account
for that, and both were designed in rather than measured:

- **Cogging torque.** A permanent-magnet hub motor has a substantial detent torque
  even open-circuit, from the magnets aligning with the stator teeth. The flow has to
  break that before anything moves at all — and a stationary turbine develops far less
  torque than a spinning one.
- **The diode bridge is a threshold.** The full bridge only conducts once the
  generated voltage exceeds roughly two forward drops, about 1.2–1.4 V. Below that
  speed the generator loads the shaft without delivering anything useful, so the
  turbine has to climb out of a dead band before the system does any work at all.

A power balance said the project should have worked. A **torque** balance at zero speed
would have predicted the failure before anything was printed. That distinction — the
system was never power-limited, it was torque-limited at startup — is the thing I
actually took away from this project, and it applies to any direct-drive machine.

## What I would do differently

- Measure the generator's cogging and no-load drag torque on the bench *first*, and
  size the turbine to exceed it at the target flow velocity, before committing to a
  geometry.
- Gear the turbine up to the generator, or choose a machine with lower detent torque.
  Direct drive at 1.5 m/s asks a great deal of a hub motor built for 20 km/h.
- Replace the diode bridge with active or Schottky rectification to shrink the dead
  band, or accept it and design the turbine to start unloaded.
- Measure torque and rotational speed, not only voltage and current. Without shaft
  speed there is no *c*<sub>p</sub>, and without *c*<sub>p</sub> the blade variants
  cannot actually be compared — which is why ten printed turbines produced no
  comparative data.

## Repository contents

```
cad/     13 STL files — 10 turbine variants, mount, housing shaft, rod
docs/    report (German), rectifier circuit diagram, photographs
```

The CAD files are printable as-is. The turbines were printed in PLA.

## License

The CAD files and photographs are released under CC BY 4.0 — use them, credit the
source. The written report remains my own academic work and includes figures cited
from third-party sources under academic fair use; please do not redistribute it as
your own.
