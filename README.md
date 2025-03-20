# StructuredMultiBlockHex-Doc
Documentation for GridPro MultiBlock Data structure

StructuredMultiBlockHex is a C++ library for handling structured multi-block hexahedral grids. This library provides functionality for grid initialization, block traversal, connectivity exploration, and file I/O.

## Features
- Load structured multi-block hex grids from a file.
- Traverse and process individual blocks.
- Access block nodes, orientation, and metadata.
- Explore connectivity via block faces.
- Extract surface elements and quad sheets.
- Write grid data back to disk.

## Installation
To use this library, include the header and link the necessary dependencies:
```sh
# Example build command
// To be done
```

## Usage

### Initializing the Grid Data Structure & Loading a Grid File
```cpp
#include "StructuredMultiBlockHex.h"

// Initialize the grid and load from file
StructuredMultiBlockHex* myGrid = new StructuredMultiBlockHex("GridFileName");
myGrid->_ReadGridFile("grid_data.dat");
```

### Looping Through Each Block
```cpp
// Iterate through all blocks in the grid
for (auto& block : myGrid->get_blocks()) {
    std::cout << "Processing Block ID: " << block.get_id() << std::endl;
}
```

### Accessing Block Nodes and Other Functions
```cpp
// Get a specific block and access its node coordinates
StructuredMultiBlockHex::block& blk = myGrid->get_blocks().front();
std::vector<double> node_coords = blk.block_node_coords;

// Access nodes using the 3D matrix representation (Coord*** C)
int i = 1, j = 2, k = 3;
coord node = blk.C[i][j][k];
std::cout << "Node at (" << i << ", " << j << ", " << k << ") = " << node << std::endl;

// Resize operation (assuming scale factor as parameter)
blk.resize(2.0);
```

### Exploring Grid Connectivity via Block Faces
```cpp
// Iterate through blocks and find neighboring blocks via faces
for (auto& block : myGrid->get_blocks()) {
    for (int i = 0; i < 6; ++i) { // Loop through all block faces
        StructuredMultiBlockHex::blockface& face = block.f[i];
        StructuredMultiBlockHex::block* neighbor = face.get_pair();
        if (neighbor) {
            std::cout << "Block has neighbor on face " << i << " with ID: " << neighbor->get_id() << std::endl;
        }
    }
}
```

### Extracting Block Faces and Surface Sheets as Quads
```cpp
// Extract surface-facing quads from block faces
std::vector<Quad> surface_quads;
for (auto& block : myGrid->get_blocks()) {
    for (int i = 0; i < 6; ++i) {
        if (block.f[i].is_surface()) {
            surface_quads.push_back(block.f[i].extract_quad());
        }
    }
}
```

### Writing the Grid Data to a File
```cpp
// Write the grid structure to a file
myGrid->write_to_file("output_grid.dat");
```
