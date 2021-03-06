AC_PREREQ(2.63)
AC_INIT([repsnapper],[1.9.0],[https://github.com/timschmidt/repsnapper])

AC_CONFIG_HEADERS([config.h])
AC_CONFIG_SRCDIR([src/repsnapper.cpp])
AC_CONFIG_MACRO_DIR([m4])
dnl AC_CONFIG_AUX_DIR([config])

AM_INIT_AUTOMAKE([1.6 dist-bzip2 foreign])

m4_ifdef([AM_SILENT_RULES],[AM_SILENT_RULES([yes])])

# Checks for programs.
AC_PROG_CC
AC_PROG_CXX
# Needed for per-target cflags, like in gnomeshell-taskpanel
AM_PROG_CC_C_O

# Initialize libtool
LT_PREREQ([2.2.6])
LT_INIT([disable-static])

GETTEXT_PACKAGE=repsnapper
AC_SUBST(GETTEXT_PACKAGE)
AC_DEFINE_UNQUOTED(GETTEXT_PACKAGE, "$GETTEXT_PACKAGE",
                   [The prefix for our gettext translation domains.])
IT_PROG_INTLTOOL(0.26)
AM_GLIB_GNU_GETTEXT

PKG_PROG_PKG_CONFIG([0.22])
PKG_CHECK_MODULES(GTKMM, [gobject-2.0 glib-2.0 gtkmm-2.4 gtkglext-1.0 gtk+-2.0])
AC_SUBST(GTKMM_CFLAGS)
AC_SUBST(GTKMM_LIBS)

dnl Look for libreprap, either in the system paths or in the path
dnl provided (defaults to --prefix)
AC_ARG_WITH(libreprap,
    AS_HELP_STRING([--with-libreprap],
       [Extra prefix in which to look for libreprap. (default is $prefix)]),
    [ LIBREPRAP_PREFIX="$withval" ], [ LIBREPRAP_PREFIX="$prefix" ])

if test "x${LIBREPRAP_PREFIX}" != xNONE; then
  xLIBREPRAP_CFLAGS="-I${LIBREPRAP_PREFIX}/include"
  xLIBREPRAP_LIBS="-L${LIBREPRAP_PREFIX}/lib -lreprap"
  DISTCHECK_LIBREPRAP_FLAGS="--with-libreprap=$LIBREPRAP_PREFIX"
else
  xLIBREPRAP_LIBS="-lreprap"
fi

OLD_CPPFLAGS="$CPPFLAGS"; CPPFLAGS="${xLIBREPRAP_CFLAGS} $CPPFLAGS"
OLD_LIBS="$LIBS"; LIBS="${xLIBREPRAP_LIBS} $LIBS"
AC_CHECK_HEADER(reprap/comms.h,
    [LIBREPRAP_CFLAGS="${xLIBREPRAP_CFLAGS}"],
    AC_MSG_ERROR([cannot find headers for libreprap]))
AC_CHECK_LIB(reprap, rr_create,
    [LIBREPRAP_LIBS="${xLIBREPRAP_LIBS}"],
    AC_MSG_ERROR([cannot find binary for libreprap]))
CPPFLAGS="$OLD_CPPFLAGS"; LIBS="$OLD_LIBS"
AC_SUBST(LIBREPRAP_CFLAGS)
AC_SUBST(LIBREPRAP_LIBS)
AC_SUBST(DISTCHECK_LIBREPRAP_FLAGS)

AC_CHECK_HEADER(GL/glut.h,
    ,
    AC_MSG_ERROR([cannot find headers for freeglut]))

AC_CONFIG_FILES([
  Makefile
  po/Makefile.in
  libraries/Makefile
  libraries/polylib/Makefile
  libraries/vmmlib/Makefile
  libraries/vmmlib/include/Makefile
  libraries/vmmlib/include/vmmlib/Makefile
  src/Makefile])
AC_OUTPUT
