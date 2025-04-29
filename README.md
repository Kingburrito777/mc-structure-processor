# Minecraft Structure Processor

**Version**: 0.2  
**Date**: August 2024 - Today  
**License**: MIT

## Overview

The Minecraft Structure Processor is a Python library designed to process and manipulate Minecraft structure files in various formats, including `.litematic`, `.schem`, `.json`, `.js`, `.nbt`, `.schematic`, and `.csv` (including compressed `.csv.gz`). It converts these files into a uniform format suitable for machine learning applications or other data processing tasks. The library supports loading structures from NumPy arrays, enabling programmatic creation and modification of Minecraft structures.
This library originally started from the `structure_processor.py` file, though it has since been built on top of a completely overhauled [Litemapy](https://github.com/SmylerMC/litemapy) repository.

Key features include:
- Loading and processing multiple Minecraft structure file formats.
- Converting structures to pandas DataFrames for analysis.
- Saving processed structures as `.litematic` files.
- Loading structures from 3D NumPy arrays with support for global block IDs and local palette indices.
- Visualizing and analyzing structure metadata and block distributions.
- Cropping, dicing, and copying structures for flexible manipulation.

## Installation

### Prerequisites
- Python 3.8 or higher
- Required Python packages:
  - `numpy`
  - `pandas`
  - `nbtlib`
  - `matplotlib` (for visualization)
  - Other dependencies as specified in `requirements.txt`

### Steps
1. Clone the repository:
   ```bash
   git clone https://github.com/yourusername/minecraft-structure-processor.git
   cd minecraft-structure-processor
   ```

2. Create a virtual environment (optional but recommended):
   ```bash
   python -m venv venv
   source venv/bin/activate  # On Windows: venv\Scripts\activate
   ```

3. Install dependencies:
   ```bash
   pip install -r requirements.txt
   ```

4. Ensure the `library` directory (containing `schematic.py`, `schematic_utils.py`, `minecraft.py`, `block_processing.py`, `storage.py`, and `structure_visualization.py`) is in your project directory. Additionaly, the `block_mapping` directory is needed for conversion of different file formats' blocks into a standardized global `BLOCK_ID`.

## Usage

### Loading and Processing a Structure File
```python
from structure_processor import StructureProcessor

# Load a .litematic file
processor = StructureProcessor("path/to/structure.litematic")
# Or create a blank processor that can be populated with numpy arrays
empty_processor = StructureProcessor()

# Convert to a pandas DataFrame
df = processor.to_dataframe()
print(df.head())

# Save as a new .litematic file
processor.save_schematic("output.litematic")
```

### Loading from a NumPy Array
```python
import numpy as np
from structure_processor import StructureProcessor

# Create a 10x10x10 structure with stone blocks
block_array = np.zeros((10, 10, 10), dtype=np.int32)
block_array[2:8, 2:8, 2:8] = 1  # Stone (global ID 1)
properties = {
    (3, 3, 3): {'facing': 'north'},
    (4, 4, 4): {'waterlogged': 'true'}
}

# Initialize processor and load from NumPy array
processor = StructureProcessor()
processor._load_from_numpy(block_array, properties, is_global=True)

# Save the structure
processor.save_schematic("numpy_output.litematic")
```

### Using a Local Palette
```python
import numpy as np
from structure_processor import StructureProcessor

# Define a palette
palette = [
    {'id': 'minecraft:stone', 'properties': {}},
    {'id': 'minecraft:oak_planks', 'properties': {}}
]

# Create a structure with palette indices
block_array = np.zeros((10, 10, 10), dtype=np.uint16)
block_array[2:8, 2:8, 2:8] = 1  # Stone (palette index 1)
block_array[3:7, 3:7, 3:7] = 2  # Oak planks (palette index 2)
properties = {'palette': palette}

# Load and save
processor = StructureProcessor()
processor._load_from_numpy(block_array, properties, is_global=False)
processor.save_schematic("palette_output.litematic")
```

### Visualizing and Analyzing Structures
```python
# Visualize the structure
processor.visualize()

# Analyze structure metadata
processor.analyze()
```

### Cropping and Dicing
```python
# Crop the structure
cropped_processor = processor.crop(x_min=2, y_min=2, z_min=2, x_max=7, y_max=7, z_max=7)

# Dice the structure into 16x16x16 cubes
diced_processors = processor.dice(cube_size=16)

# Save each diced structure
for i, diced_processor in enumerate(diced_processors):
    diced_processor.save_schematic(f"diced_output_{i}.litematic")
```

## Supported File Formats
- **Litematic** (`.litematic`): Litematica schematic files.
- **Sponge** (`.schem`): Sponge schematic files.
- **Schematica** (`.schematic`): Older Schematica format.
- **BuildPaste JSON** (`.json`): BuildPaste JSON format.
- **JavaScript** (`.js`): GrabCraft JavaScript schematics.
- **NBT** (`.nbt`): Minecraft structure NBT files.
- **CSV** (`.csv` or `.csv.gz`): Custom CSV format with optional palette optimization.

## Project Structure
```
minecraft-structure-processor/
├── library/
│   ├── schematic.py        # Core schematic and region classes
│   ├── schematic_utils.py  # Utility functions for block processing
│   ├── structure_visualization.py  # Visualization and analysis functions
│   ├── block_processing.py
│   ├── storage.py
│   ├── schematic.py
├── block_mapping/
│   ├── blocks.json
│   ├── coordinate_mapping.json # Used only for GrabCraft
│   ├── modded_block_map_full.csv
│   ├── buildpaste_blocks.csv
│   ├── legacy_conversion_table.csv
│   ├── grabcraft_blocks.csv
│   ├── transparent_blocks.csvmodded_block_map_full.csv
│   ├── modded_block_map_full.csv
├── structure_processor.py  # Main StructureProcessor class
├── requirements.txt        # Dependencies
├── README.md               # This file
└── examples/               # Example scripts and sample structures

```

## Contributing
Contributions are welcome! Please follow these steps:
1. Fork the repository.
2. Create a feature branch (`git checkout -b feature/your-feature`).
3. Commit your changes (`git commit -m "Add your feature"`).
4. Push to the branch (`git push origin feature/your-feature`).
5. Open a pull request.

Please ensure your code follows PEP 8 style guidelines and includes appropriate tests.

## License
This project is licensed under the MIT License. See the `LICENSE` file for details.

## Contact
For questions or support, please open an issue on the GitHub repository or contact the maintainer at [liamlarsen12@gmail.com].

---

*Built with ❤️ for the Minecraft community.*
