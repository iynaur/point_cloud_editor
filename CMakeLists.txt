cmake_minimum_required(VERSION 3.2)

project(point_cloud_editor)

add_definitions(-std=c++11)

find_package(Qt5 COMPONENTS
             Core
             Widgets
             Gui
             OpenGL)

find_package (PCL 1.7 REQUIRED)

set(MOC_INCS
  "include/cloudEditorWidget.h"
  "include/mainWindow.h"
  "include/denoiseParameterForm.h"
  "include/statisticsDialog.h"
)

set(RSRC
  resources/pceditor_resources.qrc
)

set(INCS ${MOC_INCS}
  "include/cloud.h"
  "include/cloudTransformTool.h"
  "include/command.h"
  "include/commandQueue.h"
  "include/common.h"
  "include/copyBuffer.h"
  "include/copyCommand.h"
  "include/cutCommand.h"
  "include/deleteCommand.h"
  "include/denoiseCommand.h"
  "include/localTypes.h"
  "include/pasteCommand.h"
  "include/select1DTool.h"
  "include/select2DTool.h"
  "include/selection.h"
  "include/selectionTransformTool.h"
  "include/statistics.h"
  "include/toolInterface.h"
  "include/trackball.h"
  "include/transformCommand.h"
)

set(SRCS
  src/main.cpp
  src/mainWindow.cpp
  src/commandQueue.cpp
  src/selection.cpp
  src/copyBuffer.cpp
  src/deleteCommand.cpp
  src/cutCommand.cpp
  src/pasteCommand.cpp
  src/cloud.cpp
  src/cloudEditorWidget.cpp
  src/cloudTransformTool.cpp
  src/select1DTool.cpp
  src/select2DTool.cpp
  src/selectionTransformTool.cpp
  src/transformCommand.cpp
  src/common.cpp
  src/denoiseCommand.cpp
  src/statistics.cpp
  src/statisticsDialog.cpp
  src/trackball.cpp
  src/denoiseParameterForm.cpp
)

qt5_wrap_cpp(MOC_SRCS ${MOC_INCS} OPTIONS -DBOOST_TT_HAS_OPERATOR_HPP_INCLUDED)
qt5_add_resources(RESOURCES_SRCS ${RSRC})

include_directories(
  "${CMAKE_CURRENT_BINARY_DIR}"
  "${CMAKE_CURRENT_SOURCE_DIR}/include"
)


ADD_EXECUTABLE("${PROJECT_NAME}" ${SRCS} ${RESOURCES_SRCS} ${MOC_SRCS} ${INCS})

target_link_libraries("${PROJECT_NAME}"
    Qt5::Widgets Qt5::OpenGL
    ${OPENGL_LIBRARIES} ${BOOST_LIBRARIES} pcl_common pcl_io pcl_filters
    ${GLUT_LIBRARY} ${OPENGL_gl_LIBRARY}
    ${OPENGL_glu_LIBRARY}
    -lGL -lGLU -lGLEW
#    -lglut
    )

#PCL
include_directories (${PCL_INCLUDE_DIRS})
link_directories    (${PCL_LIBRARY_DIRS})
add_definitions     (${PCL_DEFINITIONS})


