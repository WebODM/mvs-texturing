file (GLOB HEADERS "*.h")
file (GLOB SOURCES "[^_]*.cpp")

set(BIN texrecon)

add_executable(${BIN} ${SOURCES})
set_property(TARGET ${BIN} PROPERTY INTERPROCEDURAL_OPTIMIZATION True)
include_directories(${TBB_INCLUDE_DIRS})

set(TEXRECON_LIBS tex ${TBB_LIBRARIES} ${MVE_MVE_LIBRARY} ${MVE_UTIL_LIBRARY})
if (WIN32 AND TEXRECON_USE_MIMALLOC AND TARGET mimalloc)
    # import mimalloc first, and force a reference to keep the import
    set(TEXRECON_LIBS mimalloc ${TEXRECON_LIBS})
    set_property(TARGET ${BIN} APPEND_STRING PROPERTY LINK_FLAGS " /INCLUDE:mi_version")
endif()

target_link_libraries(${BIN} ${TEXRECON_LIBS})
install(TARGETS ${BIN} RUNTIME DESTINATION bin)
