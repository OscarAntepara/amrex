#
# Does not work if amrex is built in shared mode
#
if (NOT (ENABLE_SUNDIALS AND ENABLE_SUNDIALS_CVODE) )
  return()
endif ()

set( EXENAME  "sundials3_cvode_finterface_ex1.exe" )

add_executable( ${EXENAME} EXCLUDE_FROM_ALL "" )

target_sources( ${EXENAME} PRIVATE
   integrate_ode.f90
   main.cpp
   myfunc_F.H
   ode_mod.f90
   )

set_target_properties( ${EXENAME} PROPERTIES      
   INCLUDE_DIRECTORIES
   "${CMAKE_CURRENT_LIST_DIR};${CMAKE_CURRENT_BINARY_DIR}/mod_files"
   Fortran_MODULE_DIRECTORY
   ${CMAKE_CURRENT_BINARY_DIR}/mod_files
   RUNTIME_OUTPUT_DIRECTORY
   ${CMAKE_CURRENT_BINARY_DIR} )

target_link_libraries( ${EXENAME} amrex )

#
# Find input files 
#
file( GLOB_RECURSE inputs LIST_DIRECTORIES false  ${CMAKE_CURRENT_LIST_DIR}/input* )

#
# Copy input files to corresponding build dir
#
file( COPY ${inputs} DESTINATION ${CMAKE_CURRENT_BINARY_DIR} ) 

#
# Add to the "tutorial" target
# 
add_tutorial(${EXENAME})   

