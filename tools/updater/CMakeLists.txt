# This file is part of COMP_hack.
#
# Copyright (C) 2010-2016 COMP_hack Team <compomega@tutanota.com>
#
# This program is free software: you can redistribute it and/or modify
# it under the terms of the GNU Affero General Public License as
# published by the Free Software Foundation, either version 3 of the
# License, or (at your option) any later version.
#
# This program is distributed in the hope that it will be useful,
# but WITHOUT ANY WARRANTY; without even the implied warranty of
# MERCHANTABILITY or FITNESS FOR A PARTICULAR PURPOSE.  See the
# GNU Affero General Public License for more details.
#
# You should have received a copy of the GNU Affero General Public License
# along with this program.  If not, see <http://www.gnu.org/licenses/>.

CMAKE_MINIMUM_REQUIRED(VERSION 3.5)

# Add the headless version as well.
ADD_SUBDIRECTORY(src)

IF(WIN32)

    PROJECT(comp_updater)

    SET(CMAKE_AUTOMOC ON)
    SET(CMAKE_INCLUDE_CURRENT_DIR ON)

    FIND_PACKAGE(Qt5Widgets REQUIRED)
    FIND_PACKAGE(Qt5Network REQUIRED)
    FIND_PACKAGE(Qt5WebEngine REQUIRED)
    FIND_PACKAGE(Qt5WebEngineWidgets REQUIRED)
    FIND_PACKAGE(Qt5Xml REQUIRED)

    MESSAGE("** Configuring ${PROJECT_NAME} **")

    SET(${PROJECT_NAME}_SRCS
        src/Downloader.cpp
        src/Updater.cpp
        src/main.cpp
    )

    SET(${PROJECT_NAME}_HDRS
        src/Downloader.h
        src/Updater.h
    )

    SET(${PROJECT_NAME}_UIS
        ui/Updater.ui
    )
    qt5_wrap_ui(${PROJECT_NAME}_UI_SRCS ${${PROJECT_NAME}_UIS})

    RES_WRAP(${PROJECT_NAME}_SRCS res/updater.rc)

    ADD_EXECUTABLE(${PROJECT_NAME} WIN32 ${${PROJECT_NAME}_SRCS}
        ${${PROJECT_NAME}_HDRS} ${${PROJECT_NAME}_RCC_SRCS}
        ${${PROJECT_NAME}_UI_SRCS})

    IF(MINGW)
        SET_TARGET_PROPERTIES(${PROJECT_NAME} PROPERTIES LINK_FLAGS -mwindows)
    ENDIF(MINGW)

    SET_TARGET_PROPERTIES(${PROJECT_NAME} PROPERTIES FOLDER "Tools")

    TARGET_INCLUDE_DIRECTORIES(${PROJECT_NAME} PRIVATE
        ${LIBCOMP_INCLUDES}
        ${CMAKE_CURRENT_SOURCE_DIR}/src
        ${CMAKE_CURRENT_BINARY_DIR}
    )

    TARGET_LINK_LIBRARIES(${PROJECT_NAME} Qt5::Widgets Qt5::Network
        Qt5::WebEngineWidgets Qt5::Xml zlib)

    INSTALL(TARGETS ${PROJECT_NAME} DESTINATION bin)

ENDIF(WIN32)
