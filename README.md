# GTTraj

This is a fork of the [`trajectories`](https://github.com/tobiaskunz/trajectories) repo which is based on the article [*Time-Optimal Trajectory Generation for Path Following with Bounded Acceleration and Velocity*](https://smartech.gatech.edu/bitstream/handle/1853/44347/KunzRSS12-Trajectories.pdf) by Tobias Kunz and Mike Stilman.

I forked it to test its integration with [`ocra`](http://ocra-recipes.github.io/ocra-wbi-plugins/).

## Changes
- [x] CMake configuration for cross-platform builds
- [ ] Update for GCC/G++5 (C++11 standard)
- [x] Modify the repo structure for better install:

### repo structure
```
trajectories/
├── cmake
│   └── Modules
│       ├── AddUninstallTarget.cmake
│       ├── CMakePackageConfigHelpers.cmake
│       ├── FindEigen.cmake
│       └── InstallBasicPackageFiles.cmake
├── CMakeLists.txt
├── include
│   └── gttraj
│       ├── Path.h
│       └── Trajectory.h
├── README.md
├── scripts
│   └── matlab
│       └── plotTrajectory.m
└── src
    ├── Example.cpp
    ├── Path.cpp
    ├── Test.cpp
    └── Trajectory.cpp
```
