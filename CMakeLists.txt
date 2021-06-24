cmake_minimum_required(VERSION 3.1)
project(easy_profiler_catkin)

set(CMAKE_BUILD_TYPE Release)

find_package(catkin REQUIRED)

include(ExternalProject)

find_package(catkin_simple REQUIRED)
catkin_simple()

set(VERSION 2.1.0)

file(MAKE_DIRECTORY ${CATKIN_DEVEL_PREFIX}/include)


catkin_package(
        DEPENDS
        CATKIN_DEPENDS
        INCLUDE_DIRS
        LIBRARIES easy_profiler
)

ExternalProject_Add(easy_profiler_src
#        DOWNLOAD_COMMAND wget https://github.com/yse/easy_profiler/releases/download/v2.1.0/easy_profiler-v${VERSION}-linux-x64-libc-2.27.tar.gz
        GIT_REPOSITORY https://github.com/yse/easy_profiler.git
        GIT_TAG v${VERSION}
#        DOWNLOAD_COMMAND wget https://github.com/yse/easy_profiler/archive/refs/tags/v${VERSION}.tar.gz
#        PATCH_COMMAND tar -xzf ../v${VERSION}.tar.gz && mv ../v${VERSION}.tar.gz ../easy_profiler_src-build/
#        CONFIGURE_COMMAND cmake  --prefix=${CATKIN_DEVEL_PREFIX} ..
        CONFIGURE_COMMAND cmake
            --prefix=${CATKIN_DEVEL_PREFIX}
            -DCMAKE_INSTALL_PREFIX=${CATKIN_DEVEL_PREFIX}
            -DCMAKE_BUILD_TYPE:STRING=Release
            ../easy_profiler_src
        BUILD_COMMAND $(MAKE)
        INSTALL_COMMAND $(MAKE) install
        )

#install(DIRECTORY ${CATKIN_DEVEL_PREFIX}/include/easy_profiler
#        DESTINATION ${CATKIN_GLOBAL_INCLUDE_DESTINATION}
#        FILES_MATCHING PATTERN "*.h")
install(DIRECTORY ${CATKIN_DEVEL_PREFIX}/lib/
        DESTINATION ${CATKIN_PACKAGE_LIB_DESTINATION}
        FILES_MATCHING PATTERN "libeasy_profiler*")
cs_export(INCLUDE_DIRS ${CATKIN_DEVEL_PREFIX}/include LIBRARIES easy_profiler)


