# Tuna

A Graphic Engine and frontend (GUI) showing a controllable gauntlet,
written in C++ with OpenGL 2.0.

## Compiling

### Windows
1. Open the solution (tuna.sln) with Visual Studio 2017
2. Select "tuna-gui" as the startup project, by right-clicking on "tuna-gui" and clicking on "Set as StartUp
project"
3. Right click on the solution "Solution ’tuna’" and click on "Restore NuGet Packages"
4. Right click on the solution "Solution ’tuna’" and click "Retarget Solution"
5. Fix the debugger working directory manually, by right-clicking on each project you want to run,
select "Properties", select "Configuration Properties" -> Debugging and put "$(TargetDir)" in "Working
Directory". Apply the changes by clicking on Apply, then OK.
6. Run the project with the green arrow on the top bar

### Linux
#### Requirements
- FreeGLUT
- Google Test
- GLM
- OpenGL

(or use `docker run -v $PWD:/app -it dvitali/tuna-builder:latest sh`)

```
$ mkdir cmake-build-release
$ cd cmake-build-release/
$ cmake -DCMAKE_BUILD_TYPE=Release ..
$ make $(nproc)
$ cd tuna-gui/
$ ./tuna_gui
```

## More information
Please, take a look at the docs, in the directory `tuna-docs` of this project.

## Contributors
- Filippo Pura
- Alessandro Ferrari
- Denys Vitali