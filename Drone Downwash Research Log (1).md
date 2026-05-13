# Drone Downwash Sensing - Research Log

## Research Question

Can a drone identify the geometry and properties of nearby surfaces by sensing how its own propeller downwash reflects back, using only onboard sensors (barometer, IMU, motor telemetry) and no external cameras or additional hardware?

This work extends [Tagliabue et al. (2023)](https://www.nature.com/articles/s44182-023-00002-9), which demonstrated wall proximity detection through thrust profile changes. Our direction focuses on classifying surface geometry. Not just detecting that something is nearby, but identifying what shape it is (flat floor, wall, corner, cylinder, gap, etc.).

---

## Meeting Notes

### Meeting 1 - with Neelay

Action items:
- Message Steve for drone license (done, taking next steps)
- Read about ground effect (done, understood)
- Ask John and Andy about RIC access, find out when they go to RIC next
- Build a 3D-printed stand for the motor test stand and connect to the motor. Coordinate with Mark Delloue, check if he needs help making it
- Store all progress in a GitHub repo (done)

Reading assigned:
- Momentum theory (simplification of fluid dynamics), pg 58 onward: [Principles of Helicopter Aerodynamics](https://archive.org/details/principlesofheli0000leis)
- Nature paper on navigating without vision sensors by detecting thrust profile changes near walls: [Tagliabue et al. 2023](https://www.nature.com/articles/s44182-023-00002-9)

Key reference papers:
- Downwash modeled as a turbulent jet: [arXiv 2403.13321](https://arxiv.org/abs/2403.13321)
- Interactive perception (manipulating environment to gather information): [IEEE 2011](https://ieeexplore.ieee.org/stamp/stamp.jsp?arnumber=5980244&tag=1)

Project direction confirmed:
- Extension of the Nature paper. Same core sensing technique, applied to new geometries and surfaces
- Focus is on geometry identification first (floor, wall, corner, ceiling, obstacles at different angles). Material classification is a later stage
- Modeling object characteristics based on downwash force needed to move them (e.g. tennis ball) is left for later

---

## Technical Details

### Core concept
When a drone hovers near a surface, its propeller downwash hits that surface and reflects back. The reflected airflow pushes on the drone differently depending on the geometry below. A flat floor reflects air straight back up, a corner creates recirculation, a gap lets air pass through, a curved surface deflects it sideways. These differences show up as measurable changes in thrust force, barometric pressure, and drone acceleration/tilt.

### Physics foundation
- Drone downwash can be approximated as a turbulent jet, a well-studied fluid dynamics model with predictable velocity and force profiles at different distances
- Ground effect increases thrust when a rotor is near a surface (characterized by the ratio of hover height to rotor diameter)
- Momentum theory provides the simplified framework for relating rotor thrust to induced airflow velocity

### Drone platform
- [ModalAI Starling 2 Max](https://docs.modalai.com/starling-2-max/)
- STEP file available for 3D modeling in ANSYS

### Sensors of interest (already onboard)
- Barometer: measures air pressure changes from reflected downwash
- IMU (Inertial Measurement Unit): accelerometer and gyroscope that detect changes in acceleration, tilt, and vibration caused by aerodynamic interaction
- Motor telemetry: RPM and current draw from the ESCs, which change as the flight controller compensates for external aerodynamic forces

### Simulation tool
- ANSYS Fluent (CFD) for modeling airflow around a rotor near different surface geometries
- Start with ANSYS Student edition, check with Neelay about full CMU license for larger mesh sizes

### Motor test stand
- Located in the RIC
- A single motor and propeller mounted on a stand with a load cell (sensor that measures thrust force)
- Plan: point the prop downward, spin at fixed RPM, vary distance to a surface, and record how force and pressure change
- Start with single-rotor tests, then figure out how results extend to four-rotor (quadrotor) behavior

---

## What I've Done So Far

- [x] Messaged Steve about drone license
- [x] Read about ground effect, built basic understanding
- [x] Formulated research question and Heilmeier catechism answers
- [x] Created GitHub repo
- [ ] Read momentum theory chapter (pg 58+) from helicopter textbook
- [ ] Read Nature paper methodology in detail
- [ ] Downloaded ANSYS Student
- [ ] Visited RIC and seen the motor test stand in person
- [ ] Coordinated with Mark on 3D-printed stand

---

## Next Steps

This week:
1. Read the Nature paper carefully, focus on their methodology: how they measured thrust changes, what sensors they used, what their signal processing pipeline looked like
2. Read momentum theory chapter, understand how induced velocity relates to thrust and hover height, and the ground effect correction factor
3. Download ANSYS Student and run a basic tutorial (just get familiar with the interface, meshing, and setting up a simple airflow problem)
4. Ask Neelay about full ANSYS license through CMU
5. Visit RIC, see the motor test stand, check what instrumentation it already has (load cell? data logging?), coordinate with Mark

Next 1-2 weeks:
1. Run a simple ANSYS simulation: single rotor disc above a flat surface at 3 different heights, confirm you can see ground effect in the simulation before adding complexity
2. Plan the motor test stand experiment: decide which surface geometries to test (flat wall, flat floor, 90 degree corner, cylindrical surface, open gap), what distances to test at, and what RPM to hold constant
3. If a drone is available, do a quick sanity check: hover at different heights over a flat surface and log raw barometer + IMU data, see if any signal is visible at all

Later:
- Full ANSYS simulation with Starling 2 Max STEP file
- Systematic motor test stand experiments across multiple geometries
- Extend from single-rotor to multi-rotor analysis
- Surface material classification (beyond geometry)
- Object manipulation and characterization through downwash
