# Perspective Projection Demo

It was easy. I just pulled [OpenGL Tutorials](https://github.com/opengl-tutorials/ogl) and edited the *Colored Cube* tutorial to change the **View Matrix**.

### For Windows
- Build the project using CMake
- Open the Build directory
- Open **Perspective_Colored_Cube.sln** in Visual Studio
- Select **two_point_perspective** as startup project, and run it to see two point perspective.
- Select **three_point_perspective** as startup project, and run it to see three point perspective.

### For Mac
- Build the project using CMake
- Open the Build directory
- Open **Perspective_Colored_Cube.xcodeproj** in XCode
- Select **two_point_perspective**, and run it to see two point perspective.
- Select **three_point_perspective**, and run it to see three point perspective.

### For Linux

Building on Linux is a bit complicated. Please use an IDE like CLion or QT Creator and build and run appropriately. (I know that this can be done, I just haven't tried this out of sheer laziness)

## Other methods to implement

[Here][def] is a blog by Pomax which illustrates how to implement 3 point perspective, and in the process implemented 2 point perspective as well. But thw **View Matrix** edit method was quite easy and did the worl just well.

[def]: https://pomax.github.io/three-point-perspective/

## Images

![ Two Point Perspective ](/images/twopp.png "Two Point Perspective")
![ Three Point Perspective ](/images/threepp.png "Three Point Perspective")