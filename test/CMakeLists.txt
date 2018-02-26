cmake_minimum_required(VERSION 3.0)

project(mpl_tests)

set(ALLOW_DUPLICATE_CUSTOM_TARGETS TRUE)

enable_language(CXX)

set(CMAKE_CXX_STANDARD 11)
set(CMAKE_CXX_STANDARD_REQUIRED ON)
set(CMAKE_CXX_EXTENSIONS OFF)

find_package(MPI REQUIRED) 
set(CMAKE_CXX_COMPILER ${MPI_CXX_COMPILER})

set(CMAKE_CXX_FLAGS_DEBUG "${CMAKE_CXX_FLAGS_DEBUG} -DMPL_DEBUG")

include_directories("../")

add_executable(test_send_recv test_send_recv.cc)
set_property(TARGET test_send_recv PROPERTY CXX_STANDARD 17)
add_executable(test_isend_irecv test_isend_irecv.cc)
set_property(TARGET test_isend_irecv PROPERTY CXX_STANDARD 17)
add_executable(test_collective test_collective.cc)
add_executable(test_icollective test_icollective.cc)

add_custom_target(run_tests
        COMMAND mpiexec -n 2 ./test_send_recv
        DEPENDS test_send_recv
        COMMAND mpiexec -n 2 ./test_isend_irecv
        DEPENDS test_isend_irecv
        COMMAND mpiexec -n 8 ./test_collective
        DEPENDS test_collective
        COMMAND mpiexec -n 8 ./test_icollective
        DEPENDS test_icollective
  )