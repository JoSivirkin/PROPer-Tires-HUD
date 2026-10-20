# PROPer Tires HUD

An example Starfall chip for the **PROPer Tires** Garry's Mod addon. It shows tire data on the driver's screen: every wheel's temperature, grip, surface and skid, plus the tire setup and the vehicle's speed.

## What it shows

- **TIRES panel** — one cell per wheel, laid out like the wheels on the vehicle seen from above, with an arrow pointing forward. Wheels are grouped into axles automatically, up to 15 wheels.
- **In every cell:** temperature (blue while the tire is cold, orange near the tread's limit, red above it, with the grip lost to temperature in %), "warmer" while a tire warmer is on, grip coefficient μ, surface (or "airborne") and a skid bar.
- **SETUP panel** — speed in km/h, the Grip slider value, pressure and tread. If tires have different settings, the line says "mixed".
- **WARMER panel** (left) — tire warmers. The driver sets the target with the **Up / Down arrow keys**: OFF, 30°C, 40°C … 120°C. The panel shows the target and the status: off, heating disabled (Tire heating is off on the tires), moving (warmers only work standing still), heating, ready. The chip owner must own the wheels.

## Requirements

- PROPer Tires on the server (with the heat curves update; older versions show no heat colours).
- StarfallEx.
- Wiremod's **Adv. Entity Marker** to link the wheels.

## Setup

1. In the Starfall editor create two files **in the same folder**: `tire_hud.txt` and `tire_hud_config.txt`, and copy the files from this repository into them.
2. Spawn `tire_hud.txt` on your vehicle.
3. Link all the wheels with an Adv. Entity Marker and wire its **Entities** output to the chip's **Wheels** input.
4. Wire **Seat** to the driver's seat and **Base** to the main plate.
5. Keep the vehicle standing level while wiring: the chip works out the axles from the seat's direction.
6. Get in the seat — the panel appears at the bottom of the screen.

The console tells you how many wheels and axles the chip found and how many of them are PROPer Tires.

## Settings

Everything is in `tire_hud_config.txt`: panel position and size, fonts, which side is the front, speed units (set `P.SPEED_K = 1 / 17.6` and `P.SPEED_UNIT = "mph"` for mph) and all the labels.

## Notes

- Temperature stays at 20°C unless **Tire heating** is enabled on the tire.
- Tires are only "cold" with the **Warm-up + overheat** heat model.
- The forward arrow is drawn between the left and right wheels, so it looks best when every axle has two wheels.

## Reading tires in your own chip

The chip uses one server-side function. It returns a table, or `nil` if the prop is not a tire:

```lua
--@server
local info = wheel:getPropTireInfo()
if info then
    print(info.temp, info.grip, info.surface, info.skid)
end
```

Fields: `onGround`, `load`, `slip`, `slipAngle`, `skid`, `force`, `grip`, `surface`, `wheelSpeed`, `groundSpeed`, `temp`, `heating`, `heatModel`, `heatLimit`, `heatWindow`, `heatGrip`, `warmer`, `radius`, `width`, `pressure`, `tread`, `gripSetting`.

Tire warmers (only your own tires, only while standing still):

```lua
--@server
wheel:setPropTireWarmer(90) -- heat towards 90°C; 0 switches it off
```
