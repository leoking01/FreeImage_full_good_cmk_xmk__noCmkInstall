cmake_minimum_required(VERSION 3.10)
project(FREEIMAGE
#    LANGUAGES CXX
    VERSION 1.0.1.1   )


##########################################################################################
# XMake-related settings

# Set the output paths
if (NOT DEFINED XMAKE_BINARY_DIR)
    set(XMAKE_BINARY_DIR "${FREEIMAGE_BINARY_DIR}")
    set(CMAKE_RUNTIME_OUTPUT_DIRECTORY "${XMAKE_BINARY_DIR}/bin")
    set(CMAKE_ARCHIVE_OUTPUT_DIRECTORY "${XMAKE_BINARY_DIR}/lib")
    set(CMAKE_LIBRARY_OUTPUT_DIRECTORY "${XMAKE_BINARY_DIR}/bin")
endif()

# Set the dependencies path
if (NOT DEFINED XMAKE_DEPENDENCIES_DIR)
    set(XMAKE_DEPENDENCIES_DIR "${FREEIMAGE_SOURCE_DIR}/dependencies")
endif()


##########################################################################################
# XMake importation

if (NOT EXISTS "${XMAKE_DEPENDENCIES_DIR}/XMake/XMake.cmake")
    message(FATAL_ERROR
"Missing dependency: XMake
This distribution of FreeImage requires the following dependencies:
   - XMake
   - ZLib
They are provided as GIT submodules of this repository.
Did you forgot to execute the following commands?
   git submodule init
   git submodule update")
endif()

include("${XMAKE_DEPENDENCIES_DIR}/XMake/XMake.cmake")


##########################################################################################
# Process subdirectories

add_subdirectory(dependencies)
add_subdirectory(LibJPEG)
add_subdirectory(LibMNG)
add_subdirectory(LibOpenJPEG)
add_subdirectory(LibPNG)
add_subdirectory(LibRawLite)
add_subdirectory(LibTIFF)
add_subdirectory(OpenEXR)


##########################################################################################
# Declaration of the library

# List the header files
set(HEADERS CacheFile.h
            FreeImage.h
            FreeImageIO.h
            Plugin.h
            Quantizers.h
            ToneMapping.h
            Utilities.h
            FreeImage/PSDParser.h
            FreeImageToolkit/Filters.h
            FreeImageToolkit/Resize.h
            Metadata/FIRational.h
            Metadata/FreeImageTag.h
)

# List the source files
set(SRCS FreeImage/BitmapAccess.cpp
         FreeImage/CacheFile.cpp
         FreeImage/ColorLookup.cpp
         FreeImage/Conversion.cpp
         FreeImage/Conversion16_555.cpp
         FreeImage/Conversion16_565.cpp
         FreeImage/Conversion24.cpp
         FreeImage/Conversion32.cpp
         FreeImage/Conversion4.cpp
         FreeImage/Conversion8.cpp
         FreeImage/ConversionRGBF.cpp
         FreeImage/ConversionType.cpp
         FreeImage/FreeImage.cpp
         FreeImage/FreeImageIO.cpp
         FreeImage/GetType.cpp
         FreeImage/Halftoning.cpp
         FreeImage/J2KHelper.cpp
         FreeImage/MemoryIO.cpp
         FreeImage/MultiPage.cpp
         FreeImage/NNQuantizer.cpp
         FreeImage/PSDParser.cpp
         FreeImage/PixelAccess.cpp
         FreeImage/Plugin.cpp
         FreeImage/PluginBMP.cpp
         FreeImage/PluginCUT.cpp
         FreeImage/PluginDDS.cpp
         FreeImage/PluginEXR.cpp
         FreeImage/PluginG3.cpp
         FreeImage/PluginGIF.cpp
         FreeImage/PluginHDR.cpp
         FreeImage/PluginICO.cpp
         FreeImage/PluginIFF.cpp
         FreeImage/PluginJ2K.cpp
         FreeImage/PluginJP2.cpp
         FreeImage/PluginJPEG.cpp
         FreeImage/PluginKOALA.cpp
         FreeImage/PluginMNG.cpp
         FreeImage/PluginPCD.cpp
         FreeImage/PluginPCX.cpp
         FreeImage/PluginPFM.cpp
         FreeImage/PluginPICT.cpp
         FreeImage/PluginPNG.cpp
         FreeImage/PluginPNM.cpp
         FreeImage/PluginPSD.cpp
         FreeImage/PluginRAS.cpp
         FreeImage/PluginRAW.cpp
         FreeImage/PluginSGI.cpp
         FreeImage/PluginTARGA.cpp
         FreeImage/PluginTIFF.cpp
         FreeImage/PluginWBMP.cpp
         FreeImage/PluginXBM.cpp
         FreeImage/PluginXPM.cpp
         FreeImage/TIFFLogLuv.cpp
         FreeImage/ToneMapping.cpp
         FreeImage/WuQuantizer.cpp
         FreeImage/ZLibInterface.cpp
         FreeImage/tmoColorConvert.cpp
         FreeImage/tmoDrago03.cpp
         FreeImage/tmoFattal02.cpp
         FreeImage/tmoReinhard05.cpp
         FreeImageToolkit/BSplineRotate.cpp
         FreeImageToolkit/Background.cpp
         FreeImageToolkit/Channels.cpp
         FreeImageToolkit/ClassicRotate.cpp
         FreeImageToolkit/Colors.cpp
         FreeImageToolkit/CopyPaste.cpp
         FreeImageToolkit/Display.cpp
         FreeImageToolkit/Flip.cpp
         FreeImageToolkit/JPEGTransform.cpp
         FreeImageToolkit/MultigridPoissonSolver.cpp
         FreeImageToolkit/Rescale.cpp
         FreeImageToolkit/Resize.cpp
         Metadata/Exif.cpp
         Metadata/FIRational.cpp
         Metadata/FreeImageTag.cpp
         Metadata/IPTC.cpp
         Metadata/TagConversion.cpp
         Metadata/TagLib.cpp
         Metadata/XTIFF.cpp
)

# List the include paths
include_directories(${FREEIMAGE_SOURCE_DIR}
                    ${FREEIMAGE_SOURCE_DIR}/DeprecationManager
)

xmake_import_search_paths(OPENEXR)
xmake_import_search_paths(ZLIB)

# Declaration of the library
xmake_create_static_library(FREEIMAGE freeimage ${HEADERS} ${SRCS})

xmake_add_to_list_property(FREEIMAGE COMPILE_DEFINITIONS "OPJ_STATIC;FREEIMAGE_LIB;LIBRAW_NODLL")

if (NOT WIN32)
    xmake_add_to_property(FREEIMAGE COMPILE_FLAGS "-w")
else()
    xmake_add_to_property(FREEIMAGE COMPILE_FLAGS "/W0")
    xmake_add_to_list_property(FREEIMAGE COMPILE_DEFINITIONS "_CRT_SECURE_NO_DEPRECATE")
endif()

xmake_project_link(FREEIMAGE JPEG MNG OPENEXR OPENJPEG PNG RAWLITE TIFF ZLIB)

# Exportations
xmake_export_include_paths(FREEIMAGE ${FREEIMAGE_SOURCE_DIR})
xmake_export_compile_definitions(FREEIMAGE "FREEIMAGE_LIB" "OPJ_STATIC" "LIBRAW_NODLL")





















#-------------------install  destation ---------------------------
#set( DESTINATION  "../install/")
set(CMAKE_INSTALL_PREFIX "../install_FreeImage_VC14/")
#          And to the end of the top-level CMakeLists.txt we add:
#install(TARGETS ${PROJECT_NAME} DESTINATION bin)
#install(FILES "${PROJECT_BINARY_DIR}/TutorialConfig.h"  DESTINATION include  )


if (0)
    #
    # Format the project using the `clang-format` target (i.e: cmake --build build --target clang-format)
    #

    #add_clang_format_target()

    #
    # Install library for easy downstream inclusion
    #

    include(GNUInstallDirs)
    install(
        TARGETS
        ${PROJECT_NAME}
        EXPORT
        ${PROJECT_NAME}Targets
        LIBRARY DESTINATION
        ${CMAKE_INSTALL_LIBDIR}
        RUNTIME DESTINATION
        ${CMAKE_INSTALL_BINDIR}
        ARCHIVE DESTINATION
        ${CMAKE_INSTALL_LIBDIR}
        INCLUDES DESTINATION
        include
        PUBLIC_HEADER DESTINATION
        include
        )

    install(
        EXPORT
        ${PROJECT_NAME}Targets
        FILE
        ${PROJECT_NAME}Targets.cmake
        NAMESPACE
        ${PROJECT_NAME}::
        DESTINATION
        ${CMAKE_INSTALL_LIBDIR}/cmake/${PROJECT_NAME}
        )

    #
    # Add version header
    #

    configure_file(
        ${CMAKE_CURRENT_LIST_DIR}/cmake/version.hpp.in
        include/${PROJECT_NAME_LOWERCASE}/version.hpp
        @ONLY
        )

    install(
        FILES
        ${CMAKE_CURRENT_BINARY_DIR}/include/${PROJECT_NAME_LOWERCASE}/version.hpp
        DESTINATION
        include/${PROJECT_NAME_LOWERCASE}
        )

    #
    # Install the `include` directory
    #

    #install(
    #    DIRECTORY
    ##    include/${PROJECT_NAME_LOWERCASE}
    #    BasicDef/${PROJECT_NAME_LOWERCASE}
    #     exportApi/${PROJECT_NAME_LOWERCASE}
    #     DESTINATION
    #    include
    #    )

    install(
        FILES
        #    include/${PROJECT_NAME_LOWERCASE}
        #  inc/SOIL/image_DXT.h
        #  inc/SOIL/image_helper.h
        #  inc/SOIL/SOIL.h
        #  inc/SOIL/stb_image_aug.h
        #  inc/SOIL/stbi_DDS_aug_c.h
        #  inc/SOIL/stbi_DDS_aug.h
        Source/FreeImage.h
        #  exportApi/${PROJECT_NAME_LOWERCASE}/api_vision.h
        #     exportApi/${PROJECT_NAME_LOWERCASE}
        DESTINATION
        include/
        )


    #verbose_message("Install targets succesfully build. Install with `cmake --build <build_directory> --target install --config <build_config>`.")

    #
    # Quick `ConfigVersion.cmake` creation
    #

    include(CMakePackageConfigHelpers)
    write_basic_package_version_file(
        ${PROJECT_NAME}ConfigVersion.cmake
        VERSION
        ${PROJECT_VERSION}
        COMPATIBILITY
        SameMajorVersion
        )

    configure_package_config_file(
        ${CMAKE_CURRENT_LIST_DIR}/cmake/${PROJECT_NAME}Config.cmake.in
        ${CMAKE_CURRENT_BINARY_DIR}/${PROJECT_NAME}Config.cmake
        INSTALL_DESTINATION
        ${CMAKE_INSTALL_LIBDIR}/cmake/${PROJECT_NAME}
        )

    install(
        FILES
        ${CMAKE_CURRENT_BINARY_DIR}/${PROJECT_NAME}Config.cmake
        ${CMAKE_CURRENT_BINARY_DIR}/${PROJECT_NAME}ConfigVersion.cmake
        DESTINATION
        ${CMAKE_INSTALL_LIBDIR}/cmake/${PROJECT_NAME}
        )

    #
    # Generate export header if specified
    #

    if (${PROJECT_NAME}_GENERATE_EXPORT_HEADER)
        include(GenerateExportHeader)
        generate_export_header(${PROJECT_NAME})
        install(
            FILES
            ${PROJECT_BINARY_DIR}/${PROJECT_NAME_LOWERCASE}_export.h
            DESTINATION
            include
            )
        message(STATUS "Generated the export header `${PROJECT_NAME_LOWERCASE}_export.h` and installed it.")
    endif ()

    message(STATUS "Finished building requirements for installing the package.\n")

    #
    # Unit testing setup
    #
    if (0)
        if (${PROJECT_NAME}_ENABLE_UNIT_TESTING)
            enable_testing()
            message(STATUS "Build unit tests for the project. Tests should always be found in the test folder\n")
            add_subdirectory(test)
        endif ()
    endif ()


endif ()










