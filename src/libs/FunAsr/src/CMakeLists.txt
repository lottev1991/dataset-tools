file(GLOB_RECURSE _src *.h *.cpp)

add_library(${PROJECT_NAME} ${_src})
target_compile_options(${PROJECT_NAME} PRIVATE "-include" "cstring")
target_compile_options(${PROJECT_NAME} PRIVATE "-include" "cstdint")

target_include_directories(${PROJECT_NAME} PUBLIC .)

find_package(fftw3 CONFIG REQUIRED)
target_include_directories(${PROJECT_NAME} PUBLIC ${FFTW3_INCLUDE_DIRS})
target_link_directories(${PROJECT_NAME} PUBLIC ${FFTW3_LIBRARY_DIRS})
target_link_libraries(${PROJECT_NAME} PUBLIC fftw3 fftw3f)

include_directories(../include)
target_include_directories(${PROJECT_NAME} PUBLIC ../../onnxruntime/include)
target_link_directories(${PROJECT_NAME} PUBLIC ../../onnxruntime/lib)

if (WIN32)
    target_compile_definitions(${PROJECT_NAME} PUBLIC -D_RPASR_API_EXPORT)
else ()
    set(EXTRA_LIBS pthread)
    target_include_directories(${PROJECT_NAME} PUBLIC "/usr/local/opt/fftw/include")
    target_link_directories(${PROJECT_NAME} PUBLIC "/usr/local/opt/fftw/lib")

    target_include_directories(${PROJECT_NAME} PUBLIC "/usr/local/opt/openblas/include")
    target_link_directories(${PROJECT_NAME} PUBLIC "/usr/local/opt/openblas/lib")

    target_include_directories(${PROJECT_NAME} PUBLIC "/usr/include")
    target_link_directories(${PROJECT_NAME} PUBLIC "/usr/lib64")
    include_directories(${ONNXRUNTIME_DIR}/include)
endif ()

target_include_directories(${PROJECT_NAME} PUBLIC ../include)
target_link_libraries(${PROJECT_NAME} PUBLIC onnxruntime ${EXTRA_LIBS})



