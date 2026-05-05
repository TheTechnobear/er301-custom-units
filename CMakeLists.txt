cmake_minimum_required(VERSION 3.15)
project(er301-custom-units NONE)

# Target architecture: linux (host) or am335x (ER-301 hardware)
set(ARCH "linux" CACHE STRING "Target architecture")
set_property(CACHE ARCH PROPERTY STRINGS linux am335x)

# Build profile: testing or release
set(PROFILE "testing" CACHE STRING "Build profile")
set_property(CACHE PROFILE PROPERTY STRINGS testing release)

set(MAKE_FLAGS ARCH=${ARCH} PROFILE=${PROFILE})

# ------------------------------------------------------------------
# Helper: add a build + clean target pair for a Makefile-based project
# ------------------------------------------------------------------
function(add_make_project target_name dir)
    add_custom_target(${target_name}
        COMMAND ${CMAKE_MAKE_PROGRAM} ${MAKE_FLAGS} ${ARGN}
        WORKING_DIRECTORY ${CMAKE_SOURCE_DIR}/${dir}
        COMMENT "Building ${target_name} (${dir})"
        VERBATIM
    )
    add_custom_target(${target_name}-clean
        COMMAND ${CMAKE_MAKE_PROGRAM} clean ${MAKE_FLAGS}
        WORKING_DIRECTORY ${CMAKE_SOURCE_DIR}/${dir}
        COMMENT "Cleaning ${target_name}"
        VERBATIM
    )
endfunction()

# ------------------------------------------------------------------
# Subprojects with a top-level Makefile
# ------------------------------------------------------------------
add_make_project(er-301-units-solomine  external/er-301-units-solomine)
add_make_project(er-301-units-supernicd external/er-301-units-supernicd)
add_make_project(er-301-units-tmfset    external/er-301-units-tmfset)
add_make_project(er-301-units-yrn1      external/er-301-units-yrn1)

# ------------------------------------------------------------------
# ljwall: no top-level Makefile — build each package subdirectory
# ------------------------------------------------------------------
add_make_project(ljwall-chaoticmods external/er-301-units-ljwall/chaoticmods)
add_make_project(ljwall-oscillators external/er-301-units-ljwall/oscillators)
add_make_project(ljwall-faustian    external/er-301-units-ljwall/faustian)

add_custom_target(er-301-units-ljwall
    DEPENDS ljwall-chaoticmods ljwall-oscillators ljwall-faustian
    COMMENT "Building all ljwall units"
)
add_custom_target(er-301-units-ljwall-clean
    DEPENDS ljwall-chaoticmods-clean ljwall-oscillators-clean ljwall-faustian-clean
    COMMENT "Cleaning all ljwall units"
)

# ------------------------------------------------------------------
# Aggregate targets
# ------------------------------------------------------------------
add_custom_target(all-units
    DEPENDS
        er-301-units-solomine
        er-301-units-supernicd
        er-301-units-tmfset
        er-301-units-yrn1
        er-301-units-ljwall
    COMMENT "Building all ER-301 custom units  [ARCH=${ARCH}  PROFILE=${PROFILE}]"
)

add_custom_target(clean-all
    DEPENDS
        er-301-units-solomine-clean
        er-301-units-supernicd-clean
        er-301-units-tmfset-clean
        er-301-units-yrn1-clean
        er-301-units-ljwall-clean
    COMMENT "Cleaning all ER-301 custom units"
)
