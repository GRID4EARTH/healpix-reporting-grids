# healpix-reporting-grids

Convert data between the common HEALPix grid used in [GRID4EARTH](https://grid4earth.eu) (NESTED ordering on the
WGS84 ellipsoid) and the **reporting grids that authorities use**, such as the EEA reference grid, so that results
computed on HEALPix can go directly into official reporting, and reported data can be compared on HEALPix.

> **Status: planning (GRID4EARTH2).** This repository sets out what we intend to build. There is no code yet.
> Comments and use cases are welcome as issues.

## Why

Biodiversity, water and land data are reported on many grids: the EEA reference grid (ETRS89-LAEA, EPSG:3035) at
1, 10 and 100 km, national grids, and project-specific grids. Converting every grid directly into every other grid
does not scale, and each ad hoc conversion is done differently.

With HEALPix on the WGS84 ellipsoid as the **common reference grid**, each reporting grid needs only one pair of
conversions (to and from HEALPix). Data keep their native geometry; the reference grid is used for comparison and
conversion, never as a replacement for the original data.

## Planned scope

1. **Grid definitions**: the EEA reference grid (1, 10 and 100 km), and any regular grid in a projected coordinate
   reference system defined by origin, cell size and extent.
2. **Cell coverage**: for each reporting cell, the HEALPix cells it covers at a chosen depth, and the area fraction of
   each, using [healpix-geo](https://github.com/GRID4EARTH/healpix-geo).
3. **HEALPix to reporting grid**: aggregation with the rule stated explicitly: area-weighted mean for continuous
   variables, sum for counts, presence rules (any / area threshold) for occurrence data.
4. **Reporting grid to HEALPix**: disaggregation by inheritance, flagged as such, so that coarse reported values are
   never mistaken for fine-scale observations.
5. **Provenance and uncertainty**: every converted value records the source grid and depth, the target grid, the
   rule used and the share of each cell actually covered, in the same spirit as
   [healpix-connector](https://github.com/annefou/healpix-connector).

Resampling and storage reuse the existing GRID4EARTH tools rather than reimplementing them:
[healpix-resample](https://github.com/GRID4EARTH/healpix-resample),
[healpix-convert](https://github.com/GRID4EARTH/healpix-convert) and
[healpix-compress](https://github.com/GRID4EARTH/healpix-compress); outputs follow the same Zarr conventions.

## Conventions

- NESTED ordering only; resolution is given as `depth` (`nside = 2**depth`).
- Coordinates on the WGS84 ellipsoid via `healpix-geo` (not a spherical approximation).
- Conversions are deterministic and documented: the same input, grid and rule always give the same result.

## Related

| Tool | Role |
| --- | --- |
| [healpix-geo](https://github.com/GRID4EARTH/healpix-geo) | Ellipsoidal HEALPix primitives |
| [healpix-resample](https://github.com/GRID4EARTH/healpix-resample) | Regridding to and from HEALPix |
| [healpix-convert](https://github.com/GRID4EARTH/healpix-convert) | Dataset conversion to HEALPix Zarr |
| [healpix-compress](https://github.com/GRID4EARTH/healpix-compress) | Compression of HEALPix data |
| [healpix-analyse](https://github.com/GRID4EARTH/healpix-analyse) | Analysis operators on HEALPix |
| [healpix-connector](https://github.com/annefou/healpix-connector) | Biodiversity and environmental data on HEALPix |

## License

[Apache-2.0](LICENSE), like the other GRID4EARTH tools.
