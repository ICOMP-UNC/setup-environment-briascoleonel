cmake_minimum_required(VERSION 3.25 FATAL_ERROR) #Version minima

#Especificamos nombre, version, descripcion y los lenguajes que se van a utilizar
project(
    "lab0-setup"
    VERSION 1.2.0
    DESCRIPTION "CMake C Project Template for Operating Systems II"
    LANGUAGES C
)

#Habilita el testing
include(CTest)

#Establecer estandar de C 17 y se viene el 23 (podemos ver en cppreference.com)

set(CMAKE_C_STANDARD 17)

#Puedo enganar y ponerlo en la carpeta de include
include_directories(include)

#Definir donde va a buscar el codigo fuente
include(FetchContent)
set(UNITY_GIT_URL "https://github.com/ThrowTheSwitch/Unity.git")

set(FETCHCONTET_QUIET OFF)

FetchContent_Declare (
	Unity
	GIT_REPOSITORY ${UNITY_GIT_URL}
	SOURCE_DIR ${CMAKE_CURRENT_SOURCE_DIR}/external/Unity
	UPDATE_COMMAND ""
)


#Cmake espera a que este el Unity 
FetchContent_MakeAvailable(Unity)

#Asi busca todos los archivos .c que esten en la carpeta src
#Ventaja - No tengo que escribir los archivos uno por uno
#Desventaja - Lo mismo, se me puede escapar alguno
file(GLOB_RECURSE SOURCES "src/*.c")

#Crear ejectuable
add_executable(${PROJECT_NAME} ${SOURCES})

#Agregar librerias
#Busca un CMakeLists.txt
add_subdirectory(lib/libdyn)
add_subdirectory(lib/libsta)

#Le indica donde buscar otras cabeceras
target_include_directories(${PROJECT_NAME} PUBLIC lib/libdyn/include)
target_include_directories(${PROJECT_NAME} PUBLIC lib/libsta/include)

#Linkeo librerias
target_link_libraries(${PROJECT_NAME} StatiCLib DynamiCLib)


#Variables que se pasan mientras se corre el CMake
if(RUN_TESTS EQUAL 1 OR RUN_COVERAGE EQUAL 1)
    add_subdirectory(tests)
endif()

