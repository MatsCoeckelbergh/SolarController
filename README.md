The **SolarController** class is a Python script designed for controlling a telescope through the INDI (Instrument Neutral Distributed Interface) protocol, specifically using the PyIndi library. The script is intended for solar observation and telescope control.

## Class: `SLEW_RATES`

An enumeration class defining different slew rates for telescope movement. The rates range from `MIN` (0) to `MAX` (9).

## Class: `DIRECTIONS`

An enumeration class defining cardinal directions (NORTH, EAST, SOUTH, WEST) for telescope motion.

## Class: `SolarIndiClient`

A custom INDI client class derived from `PyIndi.BaseClient`. It is responsible for handling INDI device and property events. The SolarController initializes one so you should not have to instantiate one yourself.

- **Methods:**
    - `newDevice(self, device)`: Called when a new INDI device is detected.
    - `newProperty(self, property)`: Called when a new INDI property is detected.

## Class: `SolarController`

The main controller class for telescope operations.

- **Constructor:**
    - `__init__(self, telescope_driver: str = "EQMod Mount", wait_until_stable=True)`: Initializes the controller, connects to the INDI server, and establishes communication with the telescope.

- **Methods:**
    - `move_manually(self, NS: str = None, WE: str = None)`: Initiates manual telescope motion in specified directions.
    - `is_moving(self) -> bool`: Checks if the telescope is currently in motion.
    - `abort_motion(self)`: Aborts any ongoing telescope motion.
    - `move_telescope(self, ra: float, dec: float)`: Moves the telescope to the specified equatorial coordinates (Right Ascension and Declination).
    - `move_telescope_and_wait(self, ra: float, dec: float)`: Moves the telescope and waits for the motion to complete.
    - `get_PROPERTY(self, property: str) -> tuple`: Retrieves the current value of a specified INDI property.
    - `get_EQUATORIAL_EOD_COORD(self) -> tuple`: Retrieves the current equatorial coordinates of the telescope.
    - `get_HORIZONTAL_COORD(self) -> tuple`: Retrieves the current horizontal coordinates of the telescope.
    - `get_TARGET_EOD_COORD(self) -> tuple`: Retrieves the target equatorial coordinates of the telescope.
    - `set_TELESCOPE_SLEW_RATE(self, slew_rate: int)`: Sets the telescope slew rate. This value is only used for `move_manually`. The `move_telescope` method will ignore this value and center to the coordinates with a fixed speed.

- **Direct Access (not recommended):**
    - `set_ON_COORD_SET(self, slew, track, sync)`: Sets the ON_COORD_SET property for telescope motion.
    - `set_TELESCOPE_MOTION_NS(self, direction: str)`: Sets the telescope motion in the North-South direction.
    - `set_TELESCOPE_MOTION_WE(self, direction: str)`: Sets the telescope motion in the West-East direction.
    - `set_TELESCOPE_ABORT_MOTION(self)`: Aborts the current telescope motion.
    - `connect_telescope(self, telescopeName: str)`: Connects to the specified telescope device.

## Usage Example:

```python
if __name__ == '__main__':
    solar_ctrl = SolarController()
    solar_ctrl.set_TELESCOPE_SLEW_RATE(SLEW_RATES.MIN)
    vega = {'ra': (179.23473479 * 24.0) / 360.0, 'dec': +38.78368896}
    solar_ctrl.move_telescope(vega["ra"], vega["dec"])
    print("done")
