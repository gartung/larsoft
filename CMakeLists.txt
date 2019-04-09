# ======================================================================
#  larsoft main build file
#
#  cd .../path/to/build/directory
#  source .../path/to/larsoft/ups/setup_for_development <-d|-p>
#  buildtool [-I /install/path] -bt[i] -jN
# ======================================================================
# LarSoft is an empty product
# It sets up all of the larsoft ups products (lardata, larsim, larreco, etc.)
# ======================================================================

# if the CETBUILDTOOLS_VERSION environmental variable is defined, use cetbuildtools
if(DEFINED ENV{CETBUILDTOOLS_VERSION})

# use cmake 3.14 or later
cmake_minimum_required (VERSION 3.12...3.14 FATAL_ERROR)

project(larsoft LANGUAGES CXX C)

# cetbuildtools contains our cmake modules
SET ( CETBUILDTOOLS_VERSION $ENV{CETBUILDTOOLS_VERSION} )
IF (NOT CETBUILDTOOLS_VERSION)
    MESSAGE (FATAL_ERROR "ERROR: setup cetbuildtools to get the cmake modules")
ENDIF()

set(CMAKE_MODULE_PATH $ENV{CANVAS_ROOT_IO_DIR}/Modules
		      $ENV{CETBUILDTOOLS_DIR}/Modules
		      ${CMAKE_MODULE_PATH})

include(CetCMakeEnv)
cet_cmake_env()

# these are minimum required versions, not the actual product versions
find_ups_product( cetbuildtools )

find_ups_product( lareventdisplay )
find_ups_product( larexamples     )
find_ups_product( larana          )
find_ups_product( larreco         )
find_ups_product( larrecodnn      )
find_ups_product( larpandora      )

# useful scripts
add_subdirectory(bin)

# documentation
add_subdirectory(doc)

# release database
add_subdirectory(releaseDB)

# ups - table and config files
add_subdirectory(ups)

# packaging utility
include(UseCPack)

else() # We are using cetmodules.

cmake_minimum_required (VERSION 3.11)

project( larsoft VERSION 08.05.00 LANGUAGES NONE )

find_package(cetmodules REQUIRED)

set(fcl_dir job)
set(gdml_dir gdml)

include(CetCMakeEnv)
cet_cmake_env()

# useful scripts
add_subdirectory(bin)

# documentation
add_subdirectory(doc)

# release database
add_subdirectory(releaseDB)

# generate packageConfig
add_subdirectory(cmakeConfig)

endif()
