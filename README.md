# Obsidian Houdini

## Shelf Tools Setting

`/Users/${username}/Library/Preferences/houdini/19.5/toolbar/default.shelf`

```xml
<?xml version="1.0" encoding="UTF-8"?>
<shelfDocument>
    <!-- This file contains definitions of shelves, toolbars, and tools.
         It should not be hand-edited when it is being used by the application.
         Note, that two definitions of the same element are not allowed in
         a single file. -->

    <toolshelf name="rebelway" label="Rebelway">
        <memberTool name="connect_nodes"/>
        <memberTool name="create_obj_merge"/>
        <memberTool name="toggle_update_mode"/>
        <memberTool name="group_blast_split"/>
    </toolshelf>

    <!-- Connect Nodes: Connects the selected nodes in order, making each subsequent node an input to the first node. -->
    <tool name="connect_nodes" label="Connect Nodes" icon="PLASMA_App">
        <script scriptType="python"><![CDATA[import hou
inputs = hou.selectedNodes()[1:]
target = hou.selectedNodes()[0]
target_con = len(target.inputConnections())
for count, node in enumerate(inputs):
    target.setInput(count+target_con, node)
]]></script>
    </tool>

    <!-- Create Objectmerge: Creates an Object Merge node and connects it to the selected node's path. -->
    <tool name="create_obj_merge" label="Create Obj Merge" icon="PLASMA_App">
        <script scriptType="python"><![CDATA[import hou
sel = hou.selectedNodes()

if len(sel) == 1:
    curPath = sel[0].parent()
    objPath = sel[0].path()
    pos = sel[0].position() + hou.Vector2(0,-1)
    
    mkMerge = curPath.createNode("object_merge")
    mkMerge.setPosition(pos)
    try:
        mkMerge.setName(sel[0].name() + "_merge")
    except:
        mkMerge.setName(sel[0].name() + "_merge1")
    mkMerge.parm("xformtype").set(1)
    mkMerge.parm("objpath1").set(objPath)
]]></script>
    </tool>

    <!-- Toggle Update Mode: Toggles between Auto Update and Manual update modes in Houdini. -->
    <tool name="toggle_update_mode" label="Toggle Update Mode" icon="PLASMA_App">
        <script scriptType="python"><![CDATA[import hou
mode = hou.updateModeSetting().name()

if mode == 'AutoUpdate':
    hou.setUpdateMode(hou.updateMode.Manual)
if mode == 'Manual':
    hou.setUpdateMode(hou.updateMode.AutoUpdate)
]]></script>
    </tool>

    <!-- Group Blast Split: Creates separate blast nodes for each primitive group in the selected node's geometry. -->
    <tool name="group_blast_split" label="Group Blast Split" icon="PLASMA_App">
        <script scriptType="python"><![CDATA[import hou

def generate_unique_name(parent, base_name):
    count = 1
    unique_name = base_name
    while parent.node(unique_name):
        unique_name = "{}{}".format(base_name, count)
        count += 1
    return unique_name

selectedNode = hou.selectedNodes()[0]
geo = selectedNode.geometry()
prim_groups = geo.primGroups()

for group in prim_groups:
    blastNode = selectedNode.createOutputNode('blast')
    parent
```
