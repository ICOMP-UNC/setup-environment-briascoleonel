cmake_minimum_required(VERSION 3.25 FATAL_ERROR) #Version minima

#Se refiere al directorio donde estan los headers
include_directories(${CMAKE_CURRENT_SOURCE_DIR}/../include)

#Incluimos los test files definidos en la carpeta unit (test_unit_program.c)
file(GLOB TESTS_FILES ${CMAKE_CURRENT_SOURCE_DIR}/unit/*.c)
file(GLOB SRC_FILES ${CMAKE_CURRENT_SOURCE_DIR}/../src/logic.c)

#Linkea con el projecto Unity en external
target_link_directories(${PROJECT_NAME} PUBLIC ${CMAKE_CURRENT_SOURCE_DIR}/external/Unity/src)

if(RUN_COVERAGE)
	message("Coverage enabled")
	find_program(LCOV_PATH lcov)
		if(NOT LCOV_PATH)
		message(FATAL_ERROR "lcov not found! Aborting..")
	endif()

	find_program(GCOV_PATH gcov)
		if(NOT GCOV_PATH)
		message(FATAL_ERROR "gcov not found! Aborting..")
	endif()

	find_program(GENHTML_PATH genhtml)
		if(NOT GENHTML_PATH)
		message(FATAL_ERROR "genhtml not found! Aborting..")
	endif()

	set(CMAKE_C_FLAGS "${CMAKE_C_FLAGS} -g -fprofile-arcs -ftest-coverage --coverage")
	set(CMAKE_EXE_LINKER_FLAGS "${CMAKE_EXE_LINKER_FLAGS} -lgcov --coverage")
    endif()

#Crea ejecutable para el test
add_executable(test_${PROJECT_NAME} ${TESTS_FILES} ${SRC_FILES})

#Linkeo con Unity
target_link_libraries(test_${PROJECT_NAME} unity)

#Crea el test
add_test(NAME test_${PROJECT_NAME} COMMAND test_${PROJECT_NAME})

