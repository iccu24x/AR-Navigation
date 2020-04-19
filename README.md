# AR-Navigation-Documentation

The AR Navigation 
 
- 

<p align="center">
  <img src="https://github.com/nesseratious/AR-Navigation-Documentation/blob/master/Images/RPReplay_Final1579278581.gif" />
</p>


# SYSTEM CLASSES

## The system contains and is built on the following classes: 

### ARWorldBase

Supplementary class that is used as a root to all MapSegment classes and also stores references to them. Every new MapSegment must be attached to the ARWorldBase class and a reference to every MapSegment instance must be added to the MapSegments array. Public properties: 

- WorldTrueNorthAngel: Float - Stores the rotation shift of the target area relatinve to the true polar pole. You can get the rotation shift of yout target area using Google Maps and calculating angle of rotation relative to latitude(any vertical line). The ARWorldBase class will rotate to the given angle with a short delay when the applications starts. The delay is needed to allow attached MapSegments and it's Room's to calculate their size and position using the provided model of the target area (Static Mesh). The default value is zero.

- MapSements: Array of MapSegment - Supplementary array that contains references to it's segments in case you need it.

### MapSegment

Class that is used to define a segments of the target area's map. All FloatingIcon, Room or Vertex classes that are located in thos segment should be attached to it. Public properties: 

- Floor: Integer - Stores a floor number of the segment. The default value is zero.

- FloorDisplayedText: Text - Stores a localizable text that describes the segment's floor. Keep in mind that segments can be used not only to differentiate floors, but also to divide a complex map into more small pieces in case the target area is big enoght.  The default value is "<Enter Floor Name>".
 
- SegmentMesh: StaticMesh - Fundamental mesh that defines the area of the segment. It is used in the minimap (white colour material) and in the segment itsef (transparent material) in case you will want to do a turn-based navigation(described in the NAVIGATION section). The default value is null.

- SegmentOffset: Vector2D - Stores an X,Y offset of the segment's mesh relative to the segments's origin. The default value is (0.0, 0.0).

- SegmentHeight: Float - Stores the height of the segment's mesh in metres. For this to work, the segment's mesh should be 1 metre in height. Alternatively you can create a mesh with aby height and set SegmentHeight to 1.0. The default value is 1.0.

- SegmentScale: Float - Stores the all axes scale multiplier of the segment's mesh. The default value is (1.0, 1.0, 1.0).

- SegmentMapMaterial: Material - A supplementary material that is used to show the target's floor plan. This is only need to simplify the vertex and room arrangement on the segment. The metarial is obly shown in the editor and is hidden in the game. The default value is null.


# DEBUG MODE

Before 

<p align="center">
  <img src="https://github.com/nesseratious/AR-Navigation-Documentation/blob/master/Images/RPReplay_Final1579277884.gif" />
</p>

# NAVIGATION

The navigation in this system is **not turn-based**, this means that the whole navigation path will be displayed once the user selects a destination. 

<p align="center">
  <img src="https://github.com/nesseratious/AR-Navigation-Documentation/blob/master/Images/RPReplay_Final1579278346.gif" />
</p>


If you want to opt out of this behaviour and hide all navigation arrows that are behind the walls, you can do so by adding built-in unreal's function LineTraceByChannel to the Arrow class blueprint, where the start point will be player's camera and the end point will be world location of the Arrow instance. Depending on whether the arrow instance was hit or not, you can call SetVisibility function to hide or unhide the arrow. Do not call LineTraceByChannel every frame as this will degrade performance. Put it on timer to call only 5 times per second, you can do so by wiring a CustomEvent to your functions and adding a built-in function SetTimerByFunctionName with your CustomEvent name. The visualization of such algorithm would look like this:

<p align="center">
  <img src="https://github.com/nesseratious/AR-Navigation-Documentation/blob/master/Images/111.png" />
</p>


And yet I do not recommend using this approach as my personal user surveys have shown that such behaviour may be confusing.
