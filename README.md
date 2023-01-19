# :snake: Python-TerraSphere-Blender-Automation
Script that make a TerraSphere in Blender with Python

Here is a script that will create 1024 cubes, add them to a collection, and place them on a sphere with a radius of 40 units. The cubes will be shaded with a blue emission material and will face the 3D cursor.

To use the script, open Blender and go to the Scripting workspace. Then, create a new script and paste the following code into it:

    import bpy
    import math

    # create a collection to hold the cubes
    collection = bpy.data.collections.new("Cubes")
    bpy.context.scene.collection.children.link(collection)

    # create a material for the cubes
    material = bpy.data.materials.new("BlueEmission")
    material.use_nodes = True

    # create an emission node and set its color to blue
    emission_node = material.node_tree.nodes.new("ShaderNodeEmission")
    emission_node.inputs[0].default_value = (0, 0, 1, 1)

    # link the emission node to the material's output
    output_node = material.node_tree.nodes.get("Material Output")
    material.node_tree.links.new(emission_node.outputs[0], output_node.inputs[0])

    # create the first cube and add it to the collection
    bpy.ops.mesh.primitive_cube_add(location=(0, 0, 0))
    cube = bpy.context.object
    cube.name = "Cube"
    cube.data.materials.append(material)
    collection.objects.link(cube)

    # create 1024 copies of the first cube and add them to the collection
    for i in range(1, 1024):
        bpy.ops.object.select_all(action="DESELECT")
        cube.select_set(True)
        bpy.ops.object.duplicate()
        new_cube = bpy.context.object
        new_cube.name = f"Cube.{i:04d}"
        collection.objects.link(new_cube)

    # calculate the coordinates of the cubes on a sphere with a radius of 40 units
    # and set the location of each cube to these coordinates
    cursor_location = bpy.context.scene.cursor.location
    theta = 0
    phi = 0
    delta_theta = 2 * math.pi / 32
    delta_phi = math.pi / 16
    for obj in collection.objects:
        x = 40 * math.sin(phi) * math.cos(theta)
        y = 40 * math.sin(phi) * math.sin(theta)
        z = 40 * math.cos(phi)
        obj.location = (x, y, z)
        obj.rotation_euler = (0, 0, theta)
        obj.rotation_euler = cursor_location
        theta += delta_theta
        phi += delta_phi

Save the script and run it by pressing the "Run Script" button. This will create a collection of 1024 cubes that are placed on a sphere with a radius of 40 units, shaded with a blue emission material, and facing the 3D cursor.

## Other way

Here is a script that will create 1024 cubes, add them to a collection named "Terrasphere", and place them randomly on a sphere with a radius of 40 units. The cubes will be shaded with a blue emission material and will face the 3D cursor.

To use the script, open Blender and go to the Scripting workspace. Then, create a new script and paste the following code into it:

    import bpy
    import math
    import random

    # create a collection to hold the cubes
    collection = bpy.data.collections.new("Terrasphere")
    bpy.context.scene.collection.children.link(collection)

    # create a material for the cubes
    material = bpy.data.materials.new("BlueEmission")
    material.use_nodes = True

    # create an emission node and set its color to blue
    emission_node = material.node_tree.nodes.new("ShaderNodeEmission")
    emission_node.inputs[0].default_value = (0, 0, 1, 1)

    # link the emission node to the material's output
    output_node = material.node_tree.nodes.get("Material Output")
    material.node_tree.links.new(emission_node.outputs[0], output_node.inputs[0])

    # create the first cube and add it to the collection
    bpy.ops.mesh.primitive_cube_add(location=(0, 0, 0))
    cube = bpy.context.object
    cube.name = "Cube"
    cube.data.materials.append(material)
    collection.objects.link(cube)

    # create 1024 copies of the first cube and add them to the collection
    for i in range(1, 1024):
        bpy.ops.object.select_all(action="DESELECT")
        cube.select_set(True)
        bpy.ops.object.duplicate()
        new_cube = bpy.context.object
        new_cube.name = f"Cube.{i:04d}"
        collection.objects.link(new_cube)

    # calculate the coordinates of the cubes on a sphere with a radius of 40 units
    # and set the location of each cube to a random point on the sphere
    cursor_location = bpy.context.scene.cursor.location
    for obj in collection.objects:
        theta = random.uniform(0, 2 * math.pi)
        phi = random.uniform(0, math.pi)
        x = 40 * math.sin(phi) * math.cos(theta)
        y = 40 * math.sin(phi) * math.sin(theta)
        z = 40 * math.cos(phi)
        obj.location = (x, y, z)
        obj.rotation_euler = cursor_location

Save the script and run it by pressing the "Run Script" button. This will create a collection of 1024 cubes that are placed randomly on a sphere with a radius of 40 units, shaded with a blue emission material, and facing the 3D cursor.

Note that this script is just an example and can be modified to suit your specific needs. For example, you can change the radius of the sphere, the color of the emission material, or the number of cubes that are created.
