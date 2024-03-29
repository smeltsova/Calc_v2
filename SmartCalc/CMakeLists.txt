cmake_minimum_required(VERSION 3.5)

project(SmartCalc_v2 VERSION 0.1 LANGUAGES CXX)
find_package(QT NAMES Qt5 Qt6 REQUIRED COMPONENTS Widgets)
set(CMAKE_INCLUDE_CURRENT_DIR ON)
set(CMAKE_C_STANDARD 11)
set(CMAKE_AUTOUIC ON)
set(CMAKE_AUTOMOC ON)
set(CMAKE_AUTORCC ON)

set(CMAKE_CXX_STANDARD 17)
set(CMAKE_CXX_STANDARD_REQUIRED ON)

set(CMAKE_MODULE_PATH ${CMAKE_MODULE_PATH} "$ENV{QT5_DIR}/lib/cmake")

find_package(Qt${QT_VERSION_MAJOR} REQUIRED COMPONENTS Widgets)


set(PROJECT_SOURCES
        main.cc
        view/QCustomPlot/qcustomplot.cc
        view/QCustomPlot/qcustomplot.h
        view/credit.cc
        view/credit.h
        view/credit.ui
        view/calculator.h
        view/calculator.cc
        view/calculator.ui
        view/mainwindow.cc
        view/mainwindow.h
        view/mainwindow.ui
        controller/controllercalculator.cc
        controller/controllercalculator.h
        controller/controllercredit.cc
        controller/controllercredit.h
        model/expression.h
        model/expression.cc
        model/creditcalculator.cc
        model/creditcalculator.h
)




if(${QT_VERSION_MAJOR} GREATER_EQUAL 6)
    qt_add_executable(SmartCalc
        MANUAL_FINALIZATION
        ${PROJECT_SOURCES}
    )
# Define target properties for Android with Qt 6 as:
#    set_property(TARGET SmartCalc APPEND PROPERTY QT_ANDROID_PACKAGE_SOURCE_DIR
#                 ${CMAKE_CURRENT_SOURCE_DIR}/android)
# For more information, see https://doc.qt.io/qt-6/qt-add-executable.html#target-creation

else()
    if(ANDROID)
        add_library(tcalc SHARED
            ${PROJECT_SOURCES}
        )
# Define properties for Android with Qt 5 after find_package() calls as:
#    set(ANDROID_PACKAGE_SOURCE_DIR "${CMAKE_CURRENT_SOURCE_DIR}/android")
    else()
        add_executable(SmartCalc
            ${PROJECT_SOURCES}
        )
    endif()

endif()

#target_link_libraries(SmartCalc PRIVATE Qt${QT_VERSION_MAJOR}::Widgets  Qt5::PrintSupport)
target_link_libraries(SmartCalc PRIVATE Qt${QT_VERSION_MAJOR}::Widgets)

set_target_properties(SmartCalc PROPERTIES
    MACOSX_BUNDLE_GUI_IDENTIFIER my.example.com
    MACOSX_BUNDLE_BUNDLE_VERSION ${PROJECT_VERSION}
    MACOSX_BUNDLE_SHORT_VERSION_STRING ${PROJECT_VERSION_MAJOR}.${PROJECT_VERSION_MINOR}
    MACOSX_BUNDLE TRUE
    WIN32_EXECUTABLE TRUE
)

if(QT_VERSION_MAJOR EQUAL 6)
    qt_finalize_executable(SmartCalc)
endif()


add_compile_options("-Wall -Wextra -Wpedantic -Werror")
