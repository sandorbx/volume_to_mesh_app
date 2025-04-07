# volume_to_mesh_app
Simple GUI to batch convert volume (voxel) to mesh (surface) data 

# Software User Guide

This guide describes how to convert 3D voxel files (NRRD or TIFF) into OBJ files using our software. Follow the instructions below to set the desired parameters and start the conversion process.

---

## 1. Launching the Software

- **Step 1:** Double-click the `.exe` file to open the software.
- **Step 2:** Set the parameters in the settings panel .

---

## 2. Setting Parameters

### Input Folder
- **Input Folder:**  
  Select the folder that contains the 3D voxel files (NRRD or TIFF) that you wish to convert into OBJ files.

### Marching Cubes
This section determines how the voxels will be converted to a surface.

- **Level:**  
  - **Description:** Defines the voxel intensity threshold for isosurface determination. Voxels with intensity below this level will be ignored.
  - **Recommendations:**  
    - Set to `1` if images are already clearly segmented.
    - Set to `100` or higher (for 12-bit images with 4,096 shades) if the images are not manually segmented and contain noise.
  - **Adjustments:**  
    - Increase the value if the resulting mesh contains too many noise objects.
    - Lower the value if neuronal fibers appear too fragmented.
  - **Tip:** Use *VVDViewer* to adjust the "Threshold" slider in the "Properties" panel until the neuron surface is well demarcated without noise.

- **Spacing X, Y, Z:**  
  Set these values to match the voxel size (µm per voxel) of the image.  
  **Example:** `0.38 µm, 0.38 µm, 0.38 µm` for neurons registered onto the JRC2018 “38um_iso” brain template.

### Taubin Smoothing

This section smoothens the resulting isosurface.

- **Enable Smoothing:**  
  Check to enable smoothing.

- **Lambda:**  
  - **Default:** `0.5`  
  - **Range:** Between 0 and 1.  
  - **Function:** Determines the extent to which protruded or indented parts of the surface are smoothed.
    - `0` means no smoothing.
    - `1` means maximum smoothing.

- **Mu:**  
  - **Default:** `-0.53`  
  - **Range:** Between 0 and -1.  
  - **Function:** Reintroduces some enhancement after smoothing to preserve the original surface shape.
    - `0` means no enhancement.
    - `-1` means full restoration of the original details.
  - **Reference:** See Taubin (1995) for more details.

- **Iterations:**  
  - **Default:** `6`  
  - **Function:** Number of times the Lambda and Mu smoothing process is repeated.

### Decimation

This section reduces the size of the OBJ files by reducing the number of mesh coordinate points (or mesh polygons).

- **Enable Decimation:**  
  Check to enable decimation.

- **Reduction:**  
  - **Default:** `0.7`  
  - **Function:** Determines the level of reduction.
    - `0` means no reduction.
    - `1` removes almost all mesh points.
    - **Note:** A value of `0.7` reduces the mesh points to about 30% of the original, but too high a value might distort fine structures.

- **Method:**  
  Choose one of the following:
  - **DecimatePro:** Preserves overall shape but may smooth out fine details.
  - **QuadricDecimation:** Better preserves local structures (recommended for preserving fine branches and synaptic features).

- **Preserve Topology:**  
  Check to avoid holes or splits in the mesh.

- **Avoid Splitting:**  
  Check to prevent splitting a single object (e.g., a neuron) into several pieces.

- **Preserve Boundary Vertex:**  
  Check to ensure important mesh points, such as neuron branch tips, are not deleted.

- **Maximum Error:**  
  - **Default:** `0`  
  - **Function:** Defines the acceptable error (in µm) between the original and reduced mesh shapes.

### Normals

- **Calculate Normals:**  
  Check this option to calculate normals, the vectors perpendicular to each mesh triangle.
  - **Purpose:** Ensures proper lighting effects in 3D visualization software.
  - **Note:** While MADI automatically calculates normals, other software might require them.

### General

- **Percent of CPU Threads:**  
  - **Default:** `50`  
  - **Function:** Specifies the percentage of available CPU threads to use.
  - **Note:** Use less than 100% to prevent overloading the system and causing sluggish response.

### Flip Mesh

- **Enable Flip Mesh:**  
  Check to generate a flipped version of the image file along with the original.
  - **Filename Note:** The flipped version will have `…_FL` added to the file name.

- **Brain Size X:**  
  - **Function:** Sets the physical width of the image (in µm) to determine the midline for flipping.
  - **Determination:**  
    - Use *Fiji* to open the image and check the status bar for image dimensions.
    - **Examples:**  
      - `627.76 µm` for the JRC2018 “38um_iso” brain template.
      - `264 µm` for the “4iso” VNC template.

---

## 3. Start the Conversion Process

- **Final Step:**  
  Click **“Start Processing”** to begin the conversion of your voxel files into OBJ format.
