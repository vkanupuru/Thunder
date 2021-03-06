set(TARGET gstreamerclient)

message("Setup ${TARGET} v${VERSION}...")

add_library(${TARGET} SHARED Implementation.cpp)

target_link_libraries(${TARGET}
    PRIVATE
        ${NAMESPACE}Core::${NAMESPACE}Core
        ${GSTREAMER_LIBRARIES}
        ${GSTREAMER_BASE_LIBRARIES}
        CompileSettingsDebug::CompileSettingsDebug
        )

target_include_directories(${TARGET}
    PUBLIC
        $<BUILD_INTERFACE:${CMAKE_CURRENT_SOURCE_DIR}/..>
        $<BUILD_INTERFACE:${CMAKE_CURRENT_SOURCE_DIR}/../..>
        $<INSTALL_INTERFACE:include/${NAMESPACE}>
        ${GSTREAMER_INCLUDES}
        ${GSTREAMER_BASE_INCLUDES}
        )

set_target_properties(${TARGET} PROPERTIES
        CXX_STANDARD 11
        CXX_STANDARD_REQUIRED YES
        FRAMEWORK FALSE
        PUBLIC_HEADER "${PUBLIC_HEADERS}"
        VERSION ${VERSION}
        SOVERSION ${VERSION_MAJOR}
        )

install(
        TARGETS ${TARGET}  EXPORT ${TARGET}Targets  # for downstream dependencies
        ARCHIVE DESTINATION lib COMPONENT libs      # static lib
        LIBRARY DESTINATION lib COMPONENT libs      # shared lib
        FRAMEWORK DESTINATION bin COMPONENT libs    # for mac
)

InstallPackageConfig(
        TARGETS ${TARGET}
        DESCRIPTION "A platform gstreamer sink implementation.")

InstallCMakeConfig(TARGETS ${TARGET})
