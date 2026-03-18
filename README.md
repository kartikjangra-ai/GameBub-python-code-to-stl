# GameBub-python-code-to-stl
Repository for generating STL files from G-code using Python (Build123D). Includes source STL files, generated scripts, and reconstructed STL outputs.

📦 Contents

stl/ → Original STL files

gcode/ → Generated G-code files

scripts/ → Python scripts (Build123D)

output/ → Reconstructed STL files

⚙️ Workflow
# 1. Generate G-code from STL (using any slicer)

# 2. Input G-code into Google Antigravity
#    → Prompt it to generate Build123D Python code

# 3. Run the generated script
python generate_model.py

# 4. Output STL will be saved in /output
🔁 Pipeline Overview

STL → G-code → AI-generated Python (Build123D) → Reconstructed STL

✅ Validation

Import both original and generated STL into CAD software

Compare geometry and dimensions

Ensure zero volumetric deviation

⚠️ Notes

When using Google Antigravity:

Request fully tested, error-free Python code

Ensure compatibility with Build123D

Explicitly instruct zero volumetric difference in output

🛠️ Requirements

Python 3.x

Build123D

VS Code (recommended)

Any slicing software (e.g., Cura, PrusaSlicer)

🚀 Goal

Accurately reconstruct STL models from G-code using AI-assisted parametric modeling.
