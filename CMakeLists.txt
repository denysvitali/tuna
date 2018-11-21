cmake_minimum_required (VERSION 3.12)
project (Tuna CXX)

add_subdirectory(tuna-ge)
add_subdirectory(tuna-gui)

find_program(VALGRIND "valgrind")
if(VALGRIND)
    add_custom_target(tuna-valgrind
            COMMAND "${VALGRIND}" --tool=memcheck --leak-check=full --show-reachable=yes --num-callers=20 --track-fds=yes $<TARGET_FILE:tuna_gui>)
endif()