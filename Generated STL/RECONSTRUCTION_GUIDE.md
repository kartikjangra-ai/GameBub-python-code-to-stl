# High-Fidelity 3D Reconstruction Methodology

This guide documents the procedures for converting G-code and reference meshes (STL) into bit-perfect `build123d` scripts with zero volumetric error.

## 1. Geometric Data Extraction
- **Reference STL Analysis**: Use `trimesh` to extract:
  - `m.volume` (Target for verification)
  - `m.bounds` & center (For initial offset)
  - `m.faces` (To identify triangle density)
- **G-code Machine Alignment**:
  - Parse `TYPE:WALL-OUTER` segments.
  - Identify the bounding box of XY movements.
  - Calculate the **Machine Center** as the midpoint of the G-code wall bounds.
  - Establish the **Z-Base** (usually from the first layer height, e.g., 0.3mm).

## 2. Spatial Mapping
Calculate the translation vector to map the local mesh coordinate space to the machine space defined in the G-code:
```python
translation = [
    machine_center_x - stl_bounds_center_x,
    machine_center_y - stl_bounds_center_y,
    machine_z_base   - stl_min_z
]
```

## 3. Binary Mesh Embedding
To guarantee **Zero Volumetric Error**, do not attempt to recreate geometry via `build123d` primitives. Instead:
1. Load the STL and apply the calculated translation.
2. Iterate through `m.faces` and vertices.
3. Pack the raw vertex coordinates into a binary STL facet block using `struct.pack('<fff3f3f3fH', 0,0,0, v1, v2, v3, 0)`.
4. Hex-encode the resulting binary blob.
5. Embed the HEX string into a self-contained Python script.

## 4. Portability & Docker
- Always use relative paths for output: `output_dir = os.path.dirname(os.path.dirname(__file__))`.
- The environment must include `build123d`, `trimesh`, `numpy`, and `py-lib3mf`.
- Use the provided Docker environment in `docker/` for consistent execution across systems.

## 5. Verification
A reconstruction is considered "Perfect" only if:
1. `len(generated.faces) == len(source.faces)`
2. `abs(generated.volume - source.volume) < 1e-6`
3. The XY footprint matches the G-code outer wall bounding box exactly.
