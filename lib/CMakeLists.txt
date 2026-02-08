include_directories (${andersen_SOURCE_DIR}/include)

llvm_map_components_to_libnames(LLVM_LIBS
  Core
  Support
  Analysis
  Passes
)

set (AndersenSourceCodes
	Andersen.cpp
	AndersenAA.cpp
	ConstraintCollect.cpp
	ConstraintOptimize.cpp
	ConstraintSolving.cpp
	ExternalLibrary.cpp
	NodeFactory.cpp
)
add_library (AndersenObj OBJECT ${AndersenSourceCodes})
add_library (Andersen SHARED $<TARGET_OBJECTS:AndersenObj>)
add_library (AndersenStatic STATIC $<TARGET_OBJECTS:AndersenObj>)
target_link_libraries(Andersen PRIVATE ${LLVM_LIBS})
target_link_libraries(AndersenStatic PRIVATE ${LLVM_LIBS})