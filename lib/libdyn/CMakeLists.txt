cmake_minimum_required(VERSION 3.25 FATAL_ERROR) #Version minima

project(
    "DynamiCLib"
    VERSION 1.0.0
    DESCRIPTION "CMake C Project Template for Operating Systems II"
    LANGUAGES C
)

#Establecer estandar de C 17 y se viene el 23 (podemos ver en cppreference.com)

set(CMAKE_C_STANDARD 17)

#Tengo que hacer include de cabecera
#Diferencia entre dos include
#include <header1.h> - buscan en system path 
#include "header2.h" - busco en path especificado de forma local
#Puedo enganar y ponerlo en la carpeta de include

include_directories(include)

#Definir donde va a buscar el codigo fuente

file(GLOB_RECURSE SOURCES "src/*.c")

#Crear ejectuablr

add_library(${PROJECT_NAME} SHARED ${SOURCES}) 