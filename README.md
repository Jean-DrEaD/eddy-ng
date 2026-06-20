

# eddy-ng - my fork
## I've just added PR139 and PR150 from [`upstream`](https://github.com/vvuk/eddy-ng).

- **PR139**: Adaptive bed_mesh calibration for `EDDYNG_BED_MESH_EXPERIMENTAL`, accepts `ADAPTIVE=1` | And `ADAPTIVE_MARGIN=X.X` if eu want, normal margin is 5.0.

- **PR150**: 3D Temperature Z-Drift Calibration & Enhanced Setup/Tap Reliability
This PR introduces 3D Temperature Z-Drift Calibration for `eddy-ng`, allowing Eddy current sensors to compensate for thermal expansion and Z-drift across a range of toolhead/sensor temperatures. This implementation models the Z-drift analytically using low-degree rational and cubic polynomial fits. The resulting model coefficients are saved directly to the printer configuration and reconstructed on boot.
Additionally, this PR includes several reliability and safety enhancements to the `SETUP` wizard, coordinate handling, range clamping, and domain clipping, backed by a comprehensive unit testing suite. 
### Consult [PR#150](https://github.com/vvuk/eddy-ng/pull/150) for more infos.

---

# From Upstream:

> ***Note: October 2025 -- life has gotten quite busy lately, so I've been much slower to respond to issues and make updates. Apologies, will get back to it soon!***

eddy-ng improves the Eddy current probe support in Klipper to add accurate Z-offset setting by physically making contact with the build surface. These probes are very accurate, but suffer from drifts due to changes in conductivity in the target surface as well as changes in coil parameters as temperatures change. Instead of doing temperature compensation (which is guesswork at best), eddy-ng takes a more physical approach:

1. Calibration is performed at any temperature (cold).
2. Z-homing via the sensor happens using this calibration, regardless of current temperatures. This is a "coarse" Z-home -- it is not accurate enough for printing, but is sufficient for homing, gantry leveling, and other preparation.
3. A precise Z-offset is taken with a "tap" just before printing, with the bed at print temps and the nozzle warm (but not hot -- you don't want filament drooling or damage to your build plate).
4. At the same time as the tap, the difference between the actual height (now known after the tap) and what the sensor reads at that height is saved. This offset then gets taken into account when doing a bed mesh, because it indicates the delta (due to temperatures) between what height the sensor thinks it is vs. where it actually is.

This is a standalone `eddy-ng` repository, intended to be integrated into your own Klipper installation.

## Support

Questions? Come ask on the Sovol 3D Printers Discord at `https://discord.gg/Zg45rA52G7` in the eddy-ng forum. (Nothing Sovol-specific in `eddy-ng`, just where all this work started! You can also find the server via the Discover tab in Discord, then Sovol 3D Printers)

You can also file issues in [this `eddy-ng` github repo](https://github.com/vvuk/eddy-ng/issues).

## Installation

1. Clone this repository:

```
cd ~
git clone https://github.com/vvuk/eddy-ng
```

2. Run the install script:

```
cd ~/eddy-ng
./install.sh
```

(If your klipper isn't installed in `~/klipper`, provide the path as the first argument, i.e. `./install.sh ~/my-klipper`.)

3. Follow the rest of the full `eddy-ng` setup instructions that are [available in the wiki](https://github.com/vvuk/eddy-ng/wiki).

## Updating

Run a `git pull` and then run `./install.sh` again:

```
cd ~/eddy-ng
git pull
./install.sh
```

 