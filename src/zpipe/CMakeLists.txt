AUTO_FILES("." "src" "\\.(c|cpp|h)$")
include_directories(${DEP_INCLUDE_DIR})
link_directories(${DEP_LIBRARY_DIR})
if(MSVC)
  string(REPLACE "/MDd" "" CMAKE_CXX_FLAGS_DEBUG "${CMAKE_CXX_FLAGS_DEBUG}")
  set(CMAKE_CXX_FLAGS_DEBUG "${CMAKE_CXX_FLAGS_DEBUG} /MTd")
  string(REPLACE "/MD" "" CMAKE_CXX_FLAGS_MINSIZEREL "${CMAKE_CXX_FLAGS_MINSIZEREL}")
  set(CMAKE_CXX_FLAGS_MINSIZEREL "${CMAKE_CXX_FLAGS_MINSIZEREL} /MT")
  string(REPLACE "/MD" "" CMAKE_CXX_FLAGS_RELEASE "${CMAKE_CXX_FLAGS_RELEASE}")
  set(CMAKE_CXX_FLAGS_RELEASE "${CMAKE_CXX_FLAGS_RELEASE} /MT")
  string(REPLACE "/MD" "" CMAKE_CXX_FLAGS_RELWITHDEBINFO "${CMAKE_CXX_FLAGS_RELWITHDEBINFO}")
  set(CMAKE_CXX_FLAGS_RELWITHDEBINFO "${CMAKE_CXX_FLAGS_RELWITHDEBINFO} /MT")
endif()
ADD_EXE(zpipe "${src}")
if(WIN32)
  if(MSVC)
    target_link_libraries(zpipe zlibstatic)
    set_target_properties(zpipe PROPERTIES LINK_FLAGS "/NODEFAULTLIB:LIBCMT")
  else()
    target_link_libraries(zpipe z)
  endif()
else()
  target_link_libraries(zpipe z)
  if(CYGWIN)
    target_link_libraries(zpipe iconv)
  endif()
endif()
install(TARGETS zpipe DESTINATION bin)
