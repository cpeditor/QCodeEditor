cmake_minimum_required(VERSION 3.6)
project(QCodeEditor)

set(CMAKE_CXX_STANDARD 11)

option(BUILD_EXAMPLE "Example building required" Off)

if (${BUILD_EXAMPLE})
   message(STATUS "QCodeEditor example will be built.")
   add_subdirectory(example)
endif()

set(RESOURCES_FILE
    resources/qcodeeditor_resources.qrc
)

set(INCLUDE_FILES
    include/QHighlightRule
    include/QHighlightBlockRule
    include/QCodeEditor
    include/QCXXHighlighter
    include/QLineNumberArea
    include/QStyleSyntaxHighlighter
    include/QSyntaxStyle
    include/QGLSLCompleter
    include/QGLSLHighlighter
    include/QJavaHighlighter
    include/QJSHighlighter
    include/QLanguage
    include/QXMLHighlighter
    include/QJSONHighlighter
    include/QLuaCompleter
    include/QLuaHighlighter
    include/QPythonHighlighter
    include/internal/QHighlightRule.hpp
    include/internal/QHighlightBlockRule.hpp
    include/internal/QCodeEditor.hpp
    include/internal/QCXXHighlighter.hpp
    include/internal/QJavaHighlighter.hpp
    include/internal/QJSHighlighter.hpp
    include/internal/QLineNumberArea.hpp
    include/internal/QStyleSyntaxHighlighter.hpp
    include/internal/QSyntaxStyle.hpp
    include/internal/QGLSLCompleter.hpp
    include/internal/QGLSLHighlighter.hpp
    include/internal/QLanguage.hpp
    include/internal/QXMLHighlighter.hpp
    include/internal/QJSONHighlighter.hpp
    include/internal/QLuaCompleter.hpp
    include/internal/QLuaHighlighter.hpp
    include/internal/QPythonCompleter.hpp
    include/internal/QPythonHighlighter.hpp
)

set(SOURCE_FILES
    src/internal/QCodeEditor.cpp
    src/internal/QLineNumberArea.cpp
    src/internal/QCXXHighlighter.cpp
    src/internal/QSyntaxStyle.cpp
    src/internal/QStyleSyntaxHighlighter.cpp
    src/internal/QGLSLCompleter.cpp
    src/internal/QGLSLHighlighter.cpp
    src/internal/QJavaHighlighter.cpp
    src/internal/QJSHighlighter.cpp
    src/internal/QLanguage.cpp
    src/internal/QXMLHighlighter.cpp
    src/internal/QJSONHighlighter.cpp
    src/internal/QLuaCompleter.cpp
    src/internal/QLuaHighlighter.cpp
    src/internal/QPythonCompleter.cpp
    src/internal/QPythonHighlighter.cpp
)

# Create code for QObjects
set(CMAKE_AUTOMOC On)

# Create code from resource files
set(CMAKE_AUTORCC ON)

# Find includes in corresponding build directories
find_package(Qt5Core               CONFIG REQUIRED)
find_package(Qt5Widgets            CONFIG REQUIRED)
find_package(Qt5Gui                CONFIG REQUIRED)
find_package(KF5SyntaxHighlighting CONFIG QUIET)

if(not KF5SyntaxHighlighting_FOUND)
	message(STATUS "You do not have KF5SyntaxHighlighting Installed on your System. Read below instructions carefully")
	if(WIN32)
		message(STATUS "You'r on windows, you need to build and Install KF5SyntaxHighlighting")
		message(STATUS "Execute the scripts tools/win/install_ecm.bat and tools/win/install_ksh.bat **in order** from a command line with Administrative access to install them")
		message(STATUS "It will install Extra Cmake modules and KF5SyntaxHighlighting inside C:/Program Files (x86)/ECM")
	else()
		if(APPLE)
			message(STATUS "You'r on macOS, you need to build and install KF5SyntaxHighlighting")
		else()
			message(STATUS "You'r on Linux, you need to either install ksyntax-highlighting from your package manager or build and install it yourself")
			message(STATUS "On Arch Linux, Install syntax-highlighting package with pacman")
			message(STATUS "On Debian, Install libkf5syntax-highlighting-dev package with apt")
			message(STATUS "For Building from source follow the instructions below")
		endif()
		message(STATUS "Execute the scripts tools/unix/install_ecm.sh and tools/unix/install_ksh.sh **in order** from terminal with sudo")
		message(STATUS "It will install Extra Cmake modules and KF5SyntaxHighlighting inside /usr/local")

	endif()
	message(STATUS "You can undo the installation by deleting the above directory")
	message(FATAL_ERROR "Failed to find KF5SyntaxHighlighting library")
endif()

add_library(QCodeEditor STATIC
    ${RESOURCES_FILE}
    ${SOURCE_FILES}
    ${INCLUDE_FILES}
)

target_include_directories(QCodeEditor PUBLIC
    include
)

if(CMAKE_COMPILER_IS_GNUCXX)
    target_compile_options(QCodeEditor
		PRIVATE
		-ansi
		-pedantic
		-Wall
		-Wextra
		-Weffc++
		-Woverloaded-virtual
		-Winit-self
		-Wunreachable-code
	)
endif(CMAKE_COMPILER_IS_GNUCXX)

target_link_libraries(QCodeEditor
    Qt5::Core
    Qt5::Widgets
    Qt5::Gui
    KF5::SyntaxHighlighting
)
