# This target is conditionally included when our shield is selected
if(CONFIG_SHIELD_SPLIT36_LEFT OR CONFIG_SHIELD_SPLIT36_RIGHT)

    find_program(PATCH_EXECUTABLE patch)
    if(PATCH_EXECUTABLE)
        get_filename_component(SHIELD_ROOT_DIR "${CMAKE_CURRENT_LIST_DIR}/../../.." ABSOLUTE)
        set(PATCH_FILE "${SHIELD_ROOT_DIR}/patches/split-unencrypted.patch")
        get_filename_component(ZMK_ROOT_DIR "${ZEPHYR_BASE}/../zmk" ABSOLUTE)

        if(EXISTS "${PATCH_FILE}")
            message(WARNING "==================================================")
            message(WARNING ">>> EXECUTING SHIELD PATCH HOOK")
            message(WARNING ">>> Applying to: ${ZMK_ROOT_DIR}")

            execute_process(
                COMMAND ${PATCH_EXECUTABLE} -p1 -N -r - -i "${PATCH_FILE}"
                WORKING_DIRECTORY "${ZMK_ROOT_DIR}"
                RESULT_VARIABLE patch_result
                OUTPUT_VARIABLE patch_out
                ERROR_VARIABLE patch_err
            )

            message(WARNING ">>> Patch result: ${patch_result}")
            message(WARNING ">>> Patch output: ${patch_out}")
            if(patch_err)
                message(WARNING ">>> Patch error: ${patch_err}")
            endif()
            message(WARNING "==================================================")
        else()
            message(FATAL_ERROR ">>> Patch file not found at: ${PATCH_FILE}")
        endif()
    endif()

endif()