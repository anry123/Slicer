set(KIT ${PROJECT_NAME})

set(KIT_TEST_SRCS)
set(KIT_TEST_NAMES)
set(KIT_TEST_NAMES_CXX)
set(modulenames EventBroker ROI)
add_definitions(-DGENERIC_CXX_MODULE_TEST_SKIP_DEPENDENCIES)
SlicerMacroConfigureGenericCxxModuleTests("${modulenames}" KIT_TEST_SRCS KIT_TEST_NAMES KIT_TEST_NAMES_CXX)

set(CMAKE_TESTDRIVER_BEFORE_TESTMAIN "DEBUG_LEAKS_ENABLE_EXIT_ERROR();" )
create_test_sourcelist(Tests ${KIT}CxxTests.cxx
  ${KIT_TEST_NAMES_CXX}
  qSlicerCoreModuleTest1.cxx
  qSlicerCoreModuleFactoryTest1.cxx
  qSlicerModulePanelTest2.cxx
  EXTRA_INCLUDE vtkMRMLDebugLeaksMacro.h
  )

list(REMOVE_ITEM Tests ${KIT_TEST_NAMES_CXX})
list(APPEND Tests ${KIT_TEST_SRCS})

set(LIBRARY_NAME ${PROJECT_NAME})

add_executable(${KIT}CxxTests ${Tests})
target_link_libraries(${KIT}CxxTests ${KIT})
set_target_properties(${KIT}CxxTests PROPERTIES LABELS ${KIT})

foreach(testname ${KIT_TEST_NAMES})
  SIMPLE_TEST( ${testname} )
endforeach()
SIMPLE_TEST( qSlicerCoreModuleTest1 )
SIMPLE_TEST( qSlicerCoreModuleFactoryTest1 )
SIMPLE_TEST( qSlicerModulePanelTest2 )
