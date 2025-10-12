# Environmental System

A guide to using the Gazebo Environmental System

## Scalar Fields

Generate a temperature gradient over a 20 x 20 x 20 cube centred on the origin, with a heat source in the corner [-10, -10, -10].

The same distribution is generated for different cell counts, where `n` is the
number if cells along each axis and `n = 10, 15, 20, 25, 30, 35, 50`.

The column labels for the data files are: `timestamp,x,y,z,temperature` with samples
at `timestamp = 0` and `timestamp = 1`

Can successfully load `n=50` which has `250000` rows.

- The preload system does not appear to be the cause of the memory issue,
  for scalar fields at least.
- The data is written to file in row major order - this may be helping?
- The mesh has regular spacing (i.e. not a blockMesh)

Tests

- Shuffle the rows of the data file
  - `n = 10, 50` Does not impact result


## Vector Fields

Generate a vector field over a 20 x 20 x 20 cube centred on the origin.

The column labels for the data files are: `timestamp,x,y,z,wind_speed_x,wind_speed_y,wind_speed_z`.

Can successfully load `n=50` which has `250000` rows.



## Issues

### GUI Plugins

- Point Cloud markers have shadows

- Environment Visualization Resolution / Point Cloud does not display data unless reset
- Visualization errors from gz-sim/src/systems/environment_preload/VisualizationTool

```bash
(2025-10-13 11:25:38.436) [error] [VisualizationTool.cc:97] Data does not exist beyond this time. Not publishing new environment visualization data.
```

- Environmental sensor does not publish data after reset

#### ResampleToImage

The point cloud min and max values do not agree with data loaded into the environment?

Review data ranges in converter notebook:

```bash
# n = 40
n: 40, ux.max: 18.53, ux.min: -8.14
n: 40, uy.max: 14.65, uy.min: -16.14
n: 40, uz.max: 14.45, uz.min: -7.89

# n = 50
n: 50, ux.max: 17.44, ux.min: -7.17
n: 50, uy.max: 16.41, uy.min: -12.88
n: 50, uz.max: 13.44, uz.min: -8.22

# n = 100
n: 100, ux.max: 18.94, ux.min: -7.63
n: 100, uy.max: 16.41, uy.min: -12.88
n: 100, uz.max: 13.51, uz.min: -12.10
```

- The ranges of resampled data exported to CSV agree with ranges in ParaView


