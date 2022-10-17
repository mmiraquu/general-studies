# Hello CI C++

This repository works as a personal study environment for general software development practices. A simple Hello World in C++ works as an example.

Goals
- Refresh my memory on how basic __version control__ works with __Git__.
- Learn how pull requests, issues and the likes work at __Github__
- Start a basic __C++__ project from scratch
- Create __unit tests__ in C++
- Add automatic clean code checks (SonarCloud) 
  - Called __Static code analysis__
- Add __CI/CD automation__ pipeline (CircleCI) for building the code

I'll add notes below as I work on the setup

# CircleCI

## Build automation

The builds are done with the basic command 

```
g++ -std=c++17 src/helloworld.cpp
```

According to [LearnCpp.com](https://www.learncpp.com/), it could be useful to 

- Treat warnings as errors `-Werror`. Possible warnings would not build up and they would have to be solved immediately. 
- Disable compiler extensions `-pedantic-errors`
- Turn warning levels up `-Wall -Weffc++ -Wextra -Wsign-conversion`

I wonder if these would overlap with what SonarCloud does in the static analysis.

## Linux

Linux build is done on an [Ubuntu docker image](https://circleci.com/developer/images/image/cimg/base 'CircleCI - Images - Base').

## MacOS

MacOS builds are done on a [virtual machine](https://circleci.com/docs/using-macos/ 'CircleCI - Using macOS'). [Xcode supports building universal binaries that can be run on both x86_64 and ARM64 CPU architectures.](https://circleci.com/docs/using-macos/#xcode-cross-compilation 'CircleCI - MacOS - Xcode cross compilation')

## Windows

Windows build uses a [Windows orb](https://circleci.com/developer/orbs/orb/circleci/windows 'CircleCI - Orbs - Windows'). 

In order to build using g++, MinGW needs to be installed prior to build. The install process takes a significant amount of time. As a solution, the install can be [cached](https://circleci.com/docs/caching/ 'CircleCI - Docs - Caching') so that it doesn't have to be done on each commit. Caching the Chocolatey install directory reduced the total job time from about 2 min to 1 min. However, the resulting cache size 631MiB is larger than recommended [500MB](https://circleci.com/docs/caching/#cache-size 'CircleCI - Caching - Cache Size') and may cause problems in other ways.

Notes on cache

- The cache is stored for 15 days. 
- Jobs in one workflow can share caches.

# SonarCloud

Static analysis is set up using SonarCloud. When code and tests will be added, required test coverage must be set.