cmake_minimum_required(VERSION 3.30)

project(lua_sol_cmake_sln CXX)

add_subdirectory("lua_cmake")

set(LUA_SOL_HEADERS
    "${CMAKE_CURRENT_SOURCE_DIR}/lua_cmake/lua-5.4.7/src"
    "${CMAKE_CURRENT_SOURCE_DIR}/sol2"
)

add_library(lua_sol STATIC "lua_sol.cpp")
set_property(TARGET lua_sol PROPERTY CXX_STANDARD 20)
target_include_directories(lua_sol PRIVATE ${LUA_SOL_HEADERS})
target_link_libraries(lua_sol PRIVATE lua_static)

# Lua was built with C++
target_compile_definitions(lua_sol PRIVATE SOL_USING_CXX_LUA)

# Enabling sol safeties
target_compile_definitions(lua_sol PRIVATE SOL_ALL_SAFETIES_ON)

# Making LUA_SOL_INCLUDE available to users of this lib
get_directory_property(hasParent PARENT_DIRECTORY)
if(hasParent)
    set(LUA_SOL_INCLUDE ${LUA_SOL_HEADERS} PARENT_SCOPE)
else()
    set(LUA_SOL_INCLUDE ${LUA_SOL_HEADERS})
endif()
