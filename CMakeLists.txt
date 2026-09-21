cmake_minimum_required(VERSION 3.20.0)

find_program(PATCH_EXECUTABLE patch)
if(PATCH_EXECUTABLE AND EXISTS "${CMAKE_CURRENT_LIST_DIR}/patches/split-unencrypted.patch")
    # Determine the ZMK source root directory
    if(DEFINED zmk_DIR)
        set(ZMK_ROOT_DIR "${zmk_DIR}")
    elseif(EXISTS "${ZEPHYR_BASE}/../zmk")
        set(ZMK_ROOT_DIR "${ZEPHYR_BASE}/../zmk")
    else()
        set(ZMK_ROOT_DIR "${CMAKE_CURRENT_LIST_DIR}/../zmk")
    endif()

    message(STATUS "Applying split-unencrypted.patch to ZMK at: ${ZMK_ROOT_DIR}")

    execute_process(
        COMMAND ${PATCH_EXECUTABLE} -p1 -N -r - -i "${CMAKE_CURRENT_LIST_DIR}/patches/split-unencrypted.patch"
        WORKING_DIRECTORY "${ZMK_ROOT_DIR}"
        RESULT_VARIABLE patch_result
    )

    if(patch_result EQUAL 0)
        message(STATUS "Successfully applied split-unencrypted.patch")
    else()
        message(STATUS "Patch command returned ${patch_result} (0 means applied, 1 often means already applied)")
    endif()
endif()
