set(CMAKE_INCLUDE_CURRENT_DIR ON)

set(CMAKE_CXX_FLAGS "-std=c++11 -stdlib=libc++ -Wall -Wno-deprecated-declarations -g")

add_definitions(-I ${CMAKE_SOURCE_DIR}/)

set(TOOLBOX_SRC 
	toolbox.cpp
	mm.cpp
	loader.cpp
	rm.cpp
	os.cpp
	os_internal.cpp
	os_hfs_dispatch.cpp
	os_highlevel.cpp
	os_alias.cpp
	os_fileinfo.cpp
	qd.cpp
	sane.cpp
	saneparser.cpp
	pathnames.cpp
	utility.cpp
)


add_custom_command(
	OUTPUT saneparser.cpp
	COMMAND ragel -p -G2 -o saneparser.cpp "${CMAKE_CURRENT_SOURCE_DIR}/saneparser.rl"
	MAIN_DEPENDENCY saneparser.rl 
)

add_custom_command(
	OUTPUT pathnames.cpp
	COMMAND ragel -p -G2 -o pathnames.cpp "${CMAKE_CURRENT_SOURCE_DIR}/pathnames.rl"
	MAIN_DEPENDENCY pathnames.rl 
)

set_source_files_properties(
	saneparser.cpp pathnames.cpp
	PROPERTIES
	COMPILE_FLAGS 
	"${CMAKE_CXX_FLAGS} -Wno-unused-variable"
)

add_library(TOOLBOX_LIB ${TOOLBOX_SRC})