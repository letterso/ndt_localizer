project(ndt_omp)

find_package(OpenMP)
if (OPENMP_FOUND)
    set (CMAKE_C_FLAGS "${CMAKE_C_FLAGS} ${OpenMP_C_FLAGS}")
    set (CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} ${OpenMP_CXX_FLAGS}")
endif()

file(GLOB_RECURSE sources src/*.cpp)

# 编译静态库
add_library(${PROJECT_NAME} STATIC ${sources})
target_link_libraries(${PROJECT_NAME} PUBLIC ${PCL_LIBRARIES} OpenMP::OpenMP_CXX)
target_include_directories(${PROJECT_NAME} PUBLIC ${CMAKE_CURRENT_SOURCE_DIR}/include)

# 构建接口库
set(LIB_NAME ndt_omp_lib)
add_library(${LIB_NAME} INTERFACE)
add_library(cmake_top::${PROJECT_NAME} ALIAS ${LIB_NAME})
target_include_directories(${LIB_NAME} INTERFACE ${CMAKE_CURRENT_SOURCE_DIR}/include)
target_link_libraries(${LIB_NAME} INTERFACE ${PROJECT_NAME})

