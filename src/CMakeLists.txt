SET(SOURCE_FILES
   graph_matching_via_mp_left.cpp graph_matching_via_mp_right.cpp graph_matching_via_mp_both_sides.cpp 
   graph_matching_via_mcf_left.cpp graph_matching_via_mcf_right.cpp graph_matching_via_mcf_both_sides.cpp 
   graph_matching_via_gm_left.cpp graph_matching_via_gm_right.cpp 
   #graph_matching_via_mp_uai.cpp graph_matching_via_mcf_uai.cpp graph_matching_via_gm_uai.cpp
   hungarian_bp_left.cpp hungarian_bp_right.cpp hungarian_bp_both_sides.cpp 
   #hungarian_bp_uai.cpp

   # with frustrated cycle tightening
   graph_matching_via_mp_left_tightening.cpp graph_matching_via_mp_right_tightening.cpp graph_matching_via_mp_both_sides_tightening.cpp 
   graph_matching_via_mcf_left_tightening.cpp graph_matching_via_mcf_right_tightening.cpp graph_matching_via_mcf_both_sides_tightening.cpp 
   graph_matching_via_gm_left_tightening.cpp graph_matching_via_gm_right_tightening.cpp 
   #graph_matching_via_mp_uai_tightening.cpp graph_matching_via_mcf_uai_tightening.cpp graph_matching_via_gm_uai_tightening.cpp
   hungarian_bp_left_tightening.cpp hungarian_bp_right_tightening.cpp hungarian_bp_both_sides_tightening.cpp 
   #hungarian_bp_uai_tightening.cpp
   )

add_executable(convert_to_hdf5 convert_to_hdf5.cpp)
target_link_libraries(convert_to_hdf5 LP_MP-MRF m stdc++ pthread HDF5)

foreach( source_file ${SOURCE_FILES} )
   string( REPLACE ".cpp" "" executable_file ${source_file} )
   add_executable( ${executable_file} ${source_file})
   target_link_libraries( ${executable_file} LP_MP-MRF lingeling m stdc++ pthread )
endforeach( source_file ${SOURCE_FILES} )

# with proximal bundle
#add_executable(graph_matching_mcf_proximal_bundle graph_matching_mcf_proximal_bundle.cpp ${headers} ${sources})
#target_link_libraries( graph_matching_mcf_proximal_bundle m stdc++ pthread lgl SVM arboricity)

#add_executable(graph_matching_gm_proximal_bundle graph_matching_gm_proximal_bundle.cpp ${headers} ${sources})
#target_link_libraries( graph_matching_gm_proximal_bundle m stdc++ pthread lgl SVM arboricity)

#add_executable(graph_matching_local_subproblems_left graph_matching_local_subproblems_left.cpp ${headers} ${sources})
#target_link_libraries( graph_matching_local_subproblems_left m stdc++ pthread lgl SVM arboricity)

# with conic bundle
#add_executable(graph_matching_mcf_conic_bundle graph_matching_mcf_conic_bundle.cpp ${headers} ${sources})
#target_link_libraries( graph_matching_mcf_conic_bundle m stdc++ pthread lgl SVM arboricity cb)

if(WITH_GUROBI)
   SET(GUROBI_FILES
      # lp solver
      graph_matching_via_mp_left_tightening_gurobi.cpp graph_matching_via_mp_right_tightening_gurobi.cpp graph_matching_via_mp_both_sides_tightening_gurobi.cpp 
      graph_matching_via_mcf_left_tightening_gurobi.cpp graph_matching_via_mcf_right_tightening_gurobi.cpp graph_matching_via_mcf_both_sides_tightening_gurobi.cpp 
      graph_matching_via_gm_left_tightening_gurobi.cpp graph_matching_via_gm_right_tightening_gurobi.cpp 
      #graph_matching_via_mp_uai_tightening_gurobi.cpp graph_matching_via_mcf_uai_tightening_gurobi.cpp graph_matching_via_gm_uai_tightening_gurobi.cpp
      )

   foreach( source_file ${GUROBI_FILES} )
      string( REPLACE ".cpp" "" executable_file ${source_file} )
      add_executable( ${executable_file} ${source_file} ${headers} ${sources})
      target_link_libraries( ${executable_file} m stdc++ pthread ${GUROBI_LIBRARIES})

      target_compile_definitions(${executable_file} PUBLIC -DUSE_GUROBI)
      target_include_directories(${executable_file} PUBLIC ${GUROBI_INCLUDE_DIR})
   endforeach( source_file ${GUROBI_FILES} )
endif()
