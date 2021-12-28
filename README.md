# ![icon](./addons/ply/icons/plugin.svg) godot-ply ![icon](./addons/ply/icons/plugin.svg)
Godot plugin for in-editor box modeling.

Only tested in Godot 3.3.3. Icons are only good for dark mode.

See demos [on youtube](https://www.youtube.com/channel/UCf1IV6ABf3a4nW1wEyPwmMQ).

## Installation
- Copy the contents of the plugin directory in this repository into your `addons` folder for your godot project.
- Activate the plugin in your project settings.

## Usage
Create a ![nodeicon](./addons/ply/icons/plugin.svg) PlyInstance node in your scene, and select it.

### Editing Meshes
There are four selection modes:
- ![meshicon](./addons/ply/icons/select_mesh.svg) ` 1 ` Mesh
- ![faceicon](./addons/ply/icons/select_face.svg) ` 2 ` Face
- ![edgeicon](./addons/ply/icons/select_edge.svg) ` 3 ` Edge
- ![vertexicon](./addons/ply/icons/select_vertex.svg) ` 4 ` Vertex

There are two editor modes are toggled by the ![transformicon](./addons/ply/icons/icon_tool_move.svg) `` ` `` Transform toggle in the menu.
- Selection mode: select faces, edges, and vertices with the mouse
- Transform mode: use the gizmo to translate, rotate and scale

For an even smoother transformation experience, we recommend the excellent [Grab-Scale-Rotate for Godot](https://github.com/z1dev/gsr_for_godot) plugin. With this plugin, you will never have to enter Transform mode! Thanks to [Plugin Interop](https://github.com/jarneson/godot-plugin-interop) these two plugins are nicely integrated. Although the depth select feature of GSR conflicts with Ply's geometry selection tools.

There are tools for each selection mode:
- Mesh
    - Export to OBJ: Exports the selected mesh to an OBJ file
        - Currently just exports basic geometry, excluding normals, materials, etc
    - Quick Generators
        - Plane: Generate a two-sided unit plane
        - Cube: Generate a unit cube
    - Generate: Opens a modal for more advanced generation
        - Plane: Generate a plane with specified size and subdivisions
        - Cube: Generate a cube with specified size and subdivisions
        - Isosphere: Generate a isosphere with specified radius and subdivisions
        - Cylinder: Generate a cylinder with specified radius, depth, circle vertex count, and 

- Face
    - Select Faces
        - ![loopicon1](./addons/ply/icons/face_loop.svg) Loop: a quad loop in one direction
        - ![loopicon2](./addons/ply/icons/face_loop_2.svg) Loop: a quad loop in the other direction
    - Face Tools
        - ![extrudeicon](./addons/ply/icons/extrude_face.svg) ` ctrl-e ` Extrude: Extrudes the selected face(s) along their mean normal by 1 unit
        - Connect: Remove the two selected faces, creating a new face between edges. Tries to select an edge pairing that works.. but not always.
        - Subdivide: Subdivide a quad or a tri into 4 quads or 4 tris
        - Triangulate: Triangulates a face using an ear clipping algorithm
    - Paint Faces: Moves the selected face to the selected surface, allowing multiple materials per Ply Instance.
- Edge
    - Select Edges
        - ![loopicon](./addons/ply/icons/edge_select_loop.svg) Loop: Select an edge loop from the given edge
    - Edge Tools
        - ![loopcuticon](./addons/ply/icons/loop_cut.svg) ` ctrl-r ` Cut Loop: Add a loop cut perpindicular to the selected edge
        - ![subdivideicon](./addons/ply/icons/edge_subdivide.svg) Subdivide: Splits the selected edge into two parallel edges
        - ![collapseicon](./addons/ply/icons/edge_collapse.svg) Collapse: Collapses an edge into a single vertex at its midpoint
- Vertex
    - None, yet!

### Collisions
Collision meshes are updated automatically if there is a CollisionShape child of the PlyInstance at `$StaticBody/CollsionShape`. This is the default naming if you use the `Create Trimesh Static Body` tool.

## Details
Meshes are meant to only be oriented manifolds. Some properties:
- Each edge has one or two faces (although we generally use exactly 2)
- All of an edge's faces have compatible orientation -- that is the edge origin and destination are in opposite order for opposite faces.

Ply uses a winged edge representation for edges, but omit counterclockwise navigation:
```
omitted
left ccw        right cw
         \     /
          \   /
           \ /
            o destination
            ^
            |
left face   |   right face
            |
            o origin
           / \
          /   \
         /     \
left cw          right ccw
                 omitted
```

### Implications
Given this representation, a few limitations occur that are representable in other tools:
- One cannot abritrarily extrude edges into one-sided faces, or one edge would be incident with >2 faces.
- One cannot flip individual faces, as the faces would no longer have compatible orientation.

### Plugin Interop
Uses [godot-plugin-interop](https://github.com/jarneson/godot-plugin-interop) to allow other plugins to access it.

## Contributing

Feel free to contribute! Both issues and pull requests are very welcome.

Feel free to contact me on the [godot discord server](https://discord.gg/4JBkykG), where my name is `hints`
