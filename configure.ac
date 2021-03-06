AC_INIT([LIA_RAL], [3.0], [dev-alize@listes.univ-avignon.fr])
AM_INIT_AUTOMAKE([foreign subdir-objects])

# Checks for programs.
AC_PROG_CXX
AC_PROG_CC
AC_PROG_LN_S
AC_PROG_MAKE_SET
AC_PROG_RANLIB

AC_SUBST(OS,`uname -s`)
AC_SUBST(ARCH,`uname -m`)
AC_SUBST(ALIZE_PATH)

AC_ARG_ENABLE(stacktrace,
              [  --enable-stacktrace enable backward.hpp stacktrace (will probably only work on linux [[default=yes]] ]
              enable_stacktrace=$enableval, enable_stacktrace=yes)

# enable debug compilation
AC_ARG_ENABLE(debug, 
                [  --enable-debug         compile LIA_RAL with debug information [[default=no]] ], 
                enable_optimize=$enableval, enable_optimize=no)
if test "$enable_optimize" = "yes"; then 
        CXXFLAGS="$CXXFLAGS -ggdb -Wall"
        AC_SUBST(DEBUG,"_debug")
else
        CXXFLAGS="$CXXFLAGS -O3 -ggdb -ffast-math"
        AC_SUBST(DEBUG,"")
fi

#Enable stacktrace
if test "$enable_stacktrace" = "yes"; then
    AC_MSG_NOTICE("Stacktrace enabled")
    CPPFLAGS="$CPPFLAGS -DBACKWARD_HAS_BFD=1"
    LIBS="$LIBS -lbfd -ldl"
else
    AC_MSG_NOTICE("Stacktrace disabled")
fi

# link to LAPACK
AC_ARG_ENABLE(LAPACK, 
                [  --enable-LAPACK                link LIA_RAL to LAPACK library [[default=no]] ], 
                Lapack=$enableval, Lapack=no)
if test "$Lapack" = "yes"; then
        CXXFLAGS="$CXXFLAGS -DLAPACK"
        AC_SUBST(LIBLAPACK,"-llapacke -llapack -lrefblas -lgfortran")
else
        AC_SUBST(LIBLAPACK,"")
fi

# enable multi-threading
AC_ARG_ENABLE(MT,
                [  --enable-MT            compile LIA_RAL for multi-threading for X CPUs [[default=no]] ],
                nbT=$enableval, nbT=no)
if test "$nbT" = "yes"; then
        CXXFLAGS="$CXXFLAGS -DTHREAD"
        AC_SUBST(LIBTHREAD,"-lpthread")
else
        AC_SUBST(LIBTHREAD,"")
fi


# Checks for alize libraries.
AC_ARG_WITH([alize],
        [AS_HELP_STRING([--with-alize=PATH],
                [specify the location of the Alize library; defaults to ../alize-core])],
        [ALIZE_PATH="$withval"],
        [ALIZE_PATH="`pwd`/../alize-core"])
AC_SUBST(ALIZE_PATH)
lib_alize=0
AC_CHECK_FILE([${ALIZE_PATH}/lib/libalize_${OS}_${ARCH}${DEBUG}.a],lib_alize=-lalize_${OS}_${ARCH}${DEBUG},)
AC_CHECK_FILE([${ALIZE_PATH}/lib/libalize.a],lib_alize=-lalize,)
if [[ ${lib_alize} = 0 ]] ; then
        AC_MSG_ERROR(alize library (libalize.a or libalize_${OS}_${ARCH}${DEBUG}) not found - searched in "${ALIZE_PATH}/lib")
fi
AC_SUBST(lib_alize)


# Checks for LAPACK libraries.
AC_ARG_WITH([lapack],
        [AS_HELP_STRING([--with-lapack=PATH],
                [specify the location of the Lapack library; defaults to ../lapack])],
        [LAPACK_PATH="$withval"],
        [LAPACK_PATH="`pwd`/../lapack"])
AC_SUBST(LAPACK_PATH)


# Link with SPro (for RemoteSpkDet).
AC_ARG_WITH([spro],
            [AS_HELP_STRING([--with-spro[[=PATH]]], [link LIA_RAL with the SPro library; the path to SPro defaults to ../spro/ if not provided as an argument])],
            [SPRO_PATH="$withval"],
            [with_spro=no])
AS_IF([test "x$with_spro" != "xno"], [
        AS_IF([test "x$SPRO_PATH" = "xyes"], [
                SPRO_PATH="`pwd`/../spro"
            ])
        AC_DEFINE([SPRO])
        LIBSPRO="-lspro"
        SPRO_LIB_PATH="-L$SPRO_PATH/lib"
        AC_CHECK_HEADER([$SPRO_PATH/include/system.h], 
                        [SPRO_INCLUDE_PATH="-I$SPRO_PATH/include"], 
                        [
                            AC_CHECK_HEADER([$SPRO_PATH/system.h], 
                                            [SPRO_INCLUDE_PATH="-I$SPRO_PATH/include -I$SPRO_PATH"], 
                                            [AC_MSG_ERROR("cannot find system.h for SPro (neither in $SPRO_PATH/include nor in $SPRO_PATH)")])
                    ])
    ])
AC_SUBST(SPRO_INCLUDE_PATH)
AC_SUBST(SPRO_LIB_PATH)
AC_SUBST(LIBSPRO)


# Checks for header files.
AC_HEADER_STDC
AC_CHECK_HEADERS([fcntl.h float.h netinet/in.h stdlib.h string.h strings.h sys/time.h unistd.h])

# Checks for typedefs, structures, and compiler characteristics.
AC_HEADER_STDBOOL
AC_C_CONST
AC_C_INLINE
AC_C_VOLATILE

# Checks for library functions.
AC_FUNC_MALLOC
AC_FUNC_REALLOC
AC_FUNC_STAT
AC_FUNC_VPRINTF
AC_CHECK_FUNCS([floor memmove memset pow sqrt strcasecmp strdup])

# Create the destination directories.
AC_CONFIG_COMMANDS([mkdir], [$MKDIR_P lib bin])


AC_OUTPUT([LIA_SpkTools/Makefile
                 LIA_SpkTools/src/Makefile
                 LIA_SpkDet/Makefile
                 LIA_SpkDet/TrainWorld/Makefile
                 LIA_SpkDet/TrainWorld/src/Makefile
                 LIA_SpkDet/ComputeJFAStats/Makefile
                 LIA_SpkDet/ComputeJFAStats/src/Makefile
                 LIA_SpkDet/ComputeNorm/Makefile
                 LIA_SpkDet/ComputeNorm/src/Makefile
                 LIA_SpkDet/ComputeTest/Makefile
                 LIA_SpkDet/ComputeTest/src/Makefile
                 LIA_SpkDet/CovIntra/Makefile
                 LIA_SpkDet/CovIntra/src/Makefile
                 LIA_SpkDet/CovIntra/src/SVDLIBC/Makefile
                 LIA_SpkDet/EigenChannel/Makefile
                 LIA_SpkDet/EigenChannel/src/Makefile
                 LIA_SpkDet/EigenVoice/Makefile
                 LIA_SpkDet/EigenVoice/src/Makefile
                 LIA_SpkDet/EstimateDMatrix/Makefile
                 LIA_SpkDet/EstimateDMatrix/src/Makefile
                 LIA_SpkDet/EnergyDetector/Makefile
                 LIA_SpkDet/EnergyDetector/src/Makefile
                 LIA_SpkDet/IvExtractor/Makefile
                 LIA_SpkDet/IvExtractor/src/Makefile
                 LIA_SpkDet/IvNorm/Makefile
                 LIA_SpkDet/IvNorm/src/Makefile
                 LIA_SpkDet/IvTest/Makefile
                 LIA_SpkDet/IvTest/src/Makefile
                 LIA_SpkDet/NormFeat/Makefile
                 LIA_SpkDet/NormFeat/src/Makefile
                 LIA_SpkDet/NormFeatWindowMode/Makefile
                 LIA_SpkDet/NormFeatWindowMode/src/Makefile
                 LIA_SpkDet/PLDA/Makefile
                 LIA_SpkDet/PLDA/src/Makefile
                 LIA_SpkDet/RemoteSpkDet/Makefile
                 LIA_SpkDet/RemoteSpkDet/src/Makefile
                 LIA_SpkDet/RemoteSpkDet/Client/Makefile
                 LIA_SpkDet/ShiftedDeltaFeat/Makefile
                 LIA_SpkDet/ShiftedDeltaFeat/src/Makefile
                 LIA_SpkDet/SpkAdapt/Makefile
                 LIA_SpkDet/SpkAdapt/src/Makefile
                 LIA_SpkDet/TotalVariability/Makefile
                 LIA_SpkDet/TotalVariability/src/Makefile
                 LIA_SpkDet/TrainTarget/Makefile
                 LIA_SpkDet/TrainTarget/src/Makefile
                 LIA_Utils/Makefile
                 LIA_Utils/ExtractParams/Makefile
                 LIA_Utils/ExtractParams/src/Makefile
                 LIA_Utils/FusionScore/Makefile
                 LIA_Utils/FusionScore/src/Makefile
                 LIA_Utils/Hist/Makefile
                 LIA_Utils/Hist/src/Makefile
                 LIA_Utils/LabelFusion/Makefile
                 LIA_Utils/LabelFusion/src/Makefile
                 LIA_Utils/NAPSV/Makefile
                 LIA_Utils/NAPSV/src/Makefile
                 LIA_Utils/PolyExp/Makefile
                 LIA_Utils/PolyExp/src/Makefile
                 LIA_Utils/ReadFeatFile/Makefile
                 LIA_Utils/ReadFeatFile/src/Makefile
                 LIA_Utils/ReadModel/Makefile
                 LIA_Utils/ReadModel/src/Makefile
                 LIA_Utils/ScoreWarp/Makefile
                 LIA_Utils/ScoreWarp/src/Makefile
                 LIA_Utils/Scoring/Makefile
                 LIA_Utils/Scoring/src/Makefile
                 LIA_Utils/Svm/Makefile
                 LIA_Utils/Svm/src/Makefile
                 LIA_Utils/TimeCluster/Makefile
                 LIA_Utils/TimeCluster/src/Makefile
                 LIA_Utils/ModelToSv/Makefile
                 LIA_Utils/ModelToSv/src/Makefile
                 LIA_SpkSeg/Makefile
                 LIA_SpkSeg/AcousticSegmentation/Makefile
                 LIA_SpkSeg/AcousticSegmentation/src/Makefile
                 LIA_SpkSeg/ReSegmentation/Makefile
                 LIA_SpkSeg/ReSegmentation/src/Makefile
                 LIA_SpkSeg/Segmentation/Makefile
                 LIA_SpkSeg/Segmentation/src/Makefile
                 LIA_SpkSeg/TurnDetection/Makefile
                 LIA_SpkSeg/TurnDetection/src/Makefile
                 Makefile])
