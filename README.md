# cpm_camke_sdl2

# License: MIT

# Packages:
 - cpm-cmake/CPM.cmake v0.39.0
 - libsdl-org/SDL#release-2.30.3
 - libsdl-org/SDL_image#release-2.8.2
 - libsdl-org/SDL_ttf#release-2.22.0
 - libsdl-org/SDL_mixer#release-2.8.0
 - imgui v1.90.6

# Information:
 Set up for CMakeLists.txt with CPM.cmake. To able to download github repo and build binaries and libs.

 Note it may take a while to build.

 VS2022 tool. Note it take +2.5 GB.

 The build folder once compile is 1.70 GB.

# c/c++:
  Need to test build both?

# required:
 - VS2022
 - cmake
 - 

# Build (windows):
```
./build.bat
```
Run once for set up.

```
./compile.bat
```
Run Cmake build without need to mkdir build folder.

# run app (windows):
```
./debug_run.bat
```

# Refs:
 - https://github.com/InfiniBrains/SDL2-CPM-CMake-Example
 - 
 - https://www.youtube.com/watch?v=XfZ6WrV5Z7Y  Creating a Game Loop with C & SDL (Tutorial)
 - https://stackoverflow.com/questions/10383021/how-do-i-run-a-bat-file-from-cmake


Note if you have security protect program it error in imgui. As well other program which required compile with support third parties. 