# $FreeBSD: src/gnu/usr.bin/cc/cc_tools/Makefile,v 1.88.8.1 2009/04/15 03:14:26 kensmith Exp $
# $OpenBSD: Makefile,v 1.19 2024/02/08 20:28:54 miod Exp $

.include <bsd.own.mk>

.include "../Makefile.inc"

CFLAGS+=	-I.

CFLAGS+=	-g
CFLAGS+=	-DGENERATOR_FILE -DHAVE_CONFIG_H

# Override LIBIBERTY set by Makefile.inc, We use our own for
# build tools.
LIBIBERTY=	/usr/lib/libiberty.a

.PATH:	${GCCDIR} ${GCCLIB}/libiberty

#-----------------------------------------------------------------------
# Determine content of variables used by the target/host config files

#
# The list of headers to go into tm.h
#

TARGET_INC+=	options.h
.if ${TARGET_ARCH} == "amd64"
TARGET_INC+=	i386/biarch64.h
.endif
.if ${TARGET_ARCH} == "hppa64"
TARGET_INC+=	pa/pa64-start.h
.endif
.if ${TARGET_ARCH} == "sh"
TARGET_INC+=	sh/little.h
.endif
.if ${TARGET_ARCH} != "arm" && ${TARGET_ARCH} != "mips64" && \
    ${TARGET_ARCH} != "mips64el"
TARGET_INC+=	${GCC_CPU}/${GCC_CPU}.h
.endif
.if ${TARGET_ARCH} == "i386" || ${TARGET_ARCH} == "amd64"
TARGET_INC+=	${GCC_CPU}/unix.h
TARGET_INC+=	${GCC_CPU}/att.h
.endif
TARGET_INC+=	dbxelf.h
.if ${TARGET_ARCH} == "alpha"
TARGET_INC+=	alpha/elf.h
TARGET_INC+=	alpha/openbsd1.h
.else
TARGET_INC+=	elfos.h
.endif
.if ${TARGET_ARCH} == "arm"
TARGET_INC+=	arm/elf.h
TARGET_INC+=	arm/aout.h
TARGET_INC+=	arm/openbsd1.h
TARGET_INC+=	arm/arm.h
.endif
.if ${TARGET_ARCH} == "sh"
TARGET_INC+=	sh/elf.h
.endif
TARGET_INC+=	openbsd.h
TARGET_INC+=	openbsd-libpthread.h
TARGET_INC+=	openbsd-native.h
.if ${TARGET_ARCH} == "powerpc"
TARGET_INC+=	rs6000/secureplt.h
TARGET_INC+=	freebsd-spec.h
.endif
.if ${TARGET_ARCH} != "i386" && ${TARGET_ARCH} != "amd64"
.if exists(${GCCDIR}/config/${GCC_CPU}/sysv4.h)
TARGET_INC+=	${GCC_CPU}/sysv4.h
.endif
.endif
.if ${TARGET_ARCH} == "mips64" || ${TARGET_ARCH} == "mips64el"
TARGET_INC+=	${GCC_CPU}/${GCC_CPU}.h
TM_DEFINES= MIPS_ISA_DEFAULT=3 MIPS_ABI_DEFAULT=ABI_64
.if ${TARGET_ARCH} == "mips64el"
TM_DEFINES+= TARGET_ENDIAN_DEFAULT=0
.endif
.endif
.if ${TARGET_ARCH} == "amd64"
TARGET_INC+=	${GCC_CPU}/x86-64.h
TARGET_INC+=	${GCC_CPU}/openbsd64.h
.elif ${TARGET_ARCH} == "hppa64"
TARGET_INC+=	${GCC_CPU}/openbsd64.h
.elif ${TARGET_ARCH} == "sparc64"
TARGET_INC+=	${GCC_CPU}/openbsd64.h
.elif ${TARGET_ARCH} == "i386"
TARGET_INC+=	${GCC_CPU}/openbsdelf.h
.elif ${TARGET_ARCH} == "m88k"
TARGET_INC+=	${GCC_CPU}/openbsdelf.h
.else
TARGET_INC+=	${GCC_CPU}/openbsd.h
.endif
TARGET_INC+=	defaults.h

.for H in ${TARGET_INC}
.for D in ${GCCDIR}/config ${GCCDIR} ${.CURDIR}
.if exists($D/$H)
TARGET_INC_FILES+=     $D/$H
.endif
.endfor
.endfor

#
# gtyp includes.
#
srcdir=		${GCCDIR}
CPPLIB_H=	${GCCLIB}/libcpp/include/line-map.h \
		${GCCLIB}/libcpp/include/cpplib.h
SYMTAB_H=	${GCCLIB}/libcpp/include/symtab.h
CPP_ID_DATA_H=	${CPPLIB_H} ${GCCLIB}/libcpp/include/cpp-id-data.h
HASHTAB_H=	${GCCLIB}/include/hashtab.h
SPLAY_TREE_H=	${GCCLIB}/include/splay-tree.h
out_file=	${srcdir}/config/${GCC_CPU}/${GCC_CPU}.c
tm_file_list=	${TARGET_INC_FILES}
host_xm_file_list= ${.CURDIR}/auto-host.h ${GCCLIB}/include/ansidecl.h
GTFILES_SRCDIR=	${srcdir}

# Copied unchanged from gcc/Makefile.in
GTFILES = $(srcdir)/input.h $(srcdir)/coretypes.h \
  $(CPP_ID_DATA_H) $(host_xm_file_list) \
  $(tm_file_list) $(HASHTAB_H) $(SPLAY_TREE_H) $(srcdir)/bitmap.h \
  $(srcdir)/coverage.c $(srcdir)/rtl.h \
  $(srcdir)/optabs.h $(srcdir)/tree.h $(srcdir)/function.h $(srcdir)/libfuncs.h $(SYMTAB_H) \
  $(srcdir)/real.h $(srcdir)/varray.h $(srcdir)/insn-addr.h $(srcdir)/hwint.h \
  $(srcdir)/ipa-reference.h $(srcdir)/output.h \
  $(srcdir)/cselib.h $(srcdir)/basic-block.h  $(srcdir)/cgraph.h \
  $(srcdir)/c-common.h $(srcdir)/c-tree.h $(srcdir)/reload.h \
  $(srcdir)/alias.c $(srcdir)/bitmap.c $(srcdir)/cselib.c $(srcdir)/cgraph.c \
  $(srcdir)/ipa-prop.c $(srcdir)/ipa-cp.c $(srcdir)/ipa-inline.c \
  $(srcdir)/dbxout.c $(srcdir)/dwarf2out.c $(srcdir)/dwarf2asm.c \
  $(srcdir)/dojump.c $(srcdir)/tree-profile.c \
  $(srcdir)/emit-rtl.c $(srcdir)/except.c $(srcdir)/explow.c $(srcdir)/expr.c \
  $(srcdir)/function.c $(srcdir)/except.h \
  $(srcdir)/gcse.c $(srcdir)/integrate.c $(srcdir)/lists.c $(srcdir)/optabs.c \
  $(srcdir)/profile.c $(srcdir)/regclass.c \
  $(srcdir)/reg-stack.c $(srcdir)/cfglayout.c \
  $(srcdir)/sdbout.c $(srcdir)/stor-layout.c \
  $(srcdir)/stringpool.c $(srcdir)/tree.c $(srcdir)/varasm.c \
  $(srcdir)/tree-mudflap.c $(srcdir)/tree-flow.h \
  $(srcdir)/c-objc-common.c $(srcdir)/c-common.c $(srcdir)/c-parser.c \
  $(srcdir)/tree-ssanames.c $(srcdir)/tree-eh.c $(srcdir)/tree-ssa-address.c \
  $(srcdir)/tree-phinodes.c $(srcdir)/tree-cfg.c \
  $(srcdir)/tree-dfa.c $(srcdir)/tree-ssa-propagate.c \
  $(srcdir)/tree-iterator.c $(srcdir)/gimplify.c \
  $(srcdir)/tree-chrec.h $(srcdir)/tree-vect-generic.c \
  $(srcdir)/tree-ssa-operands.h $(srcdir)/tree-ssa-operands.c \
  $(srcdir)/tree-profile.c $(srcdir)/tree-nested.c \
  $(srcdir)/ipa-reference.c $(srcdir)/tree-ssa-structalias.h \
  $(srcdir)/tree-ssa-structalias.c \
  $(srcdir)/c-pragma.h $(srcdir)/omp-low.c \
  $(srcdir)/targhooks.c $(srcdir)/cgraphunit.c $(out_file) \

# The list of frontend directories to look into
GTFILES_LANG_DIR_NAMES=		cp objc

# The list of language specific files for gengtype
.for L in ${GTFILES_LANG_DIR_NAMES} c
.if exists(${GCCDIR}/$L-config-lang.in)
# Source the language config file
L_GTFILES!=	sh -c '. ${GCCDIR}/$L-config-lang.in; echo $$gtfiles'
.else
L_GTFILES!=	sh -c '. ${GCCDIR}/$L/config-lang.in; echo $$gtfiles'
.endif
.for F in ${L_GTFILES}
GTFILES_FILES+=	$F
GTFILES_LANGS+= $L
.endfor
.endfor
GTFILES+=	${GTFILES_FILES}

#
# Tree definition files.
#
TREE_DEF_FILES=		cp/cp-tree.def objc/objc-tree.def

#
# Option files.
#
OPT_FILES=	c.opt common.opt

.if exists(${GCCDIR}/config/${GCC_CPU}/${GCC_CPU}.opt)
OPT_FILES+=	${GCCDIR}/config/${GCC_CPU}/${GCC_CPU}.opt
.endif

.if exists(${.CURDIR}/${GCC_CPU}-openbsd.opt)
OPT_FILES+=	${.CURDIR}/${GCC_CPU}-openbsd.opt
.endif

.if ${TARGET_ARCH} == "powerpc"
OPT_FILES+=	${GCCDIR}/config/${GCC_CPU}/sysv4.opt
.endif

.if ${TARGET_ARCH} == "sparc64"
OPT_FILES+=	${GCCDIR}/config/${GCC_CPU}/long-double-switch.opt
.endif

.if exists(${.CURDIR}/openbsd.opt)
OPT_FILES+=	${.CURDIR}/openbsd.opt
.endif

#-----------------------------------------------------------------------
# Build rules for header files and generator tools

# Host config
config.h:
	TARGET_CPU_DEFAULT="${TARGET_CPU_DEFAULT}" \
	HEADERS="auto-host.h ansidecl.h" \
	DEFINES="" \
	/bin/sh ${GCCDIR}/mkconfig.sh ${.TARGET}

GENSRCS+=	config.h
CLEANFILES+=	cs-config.h

# Build config
bconfig.h:
	TARGET_CPU_DEFAULT="${TARGET_CPU_DEFAULT}" \
	HEADERS="auto-host.h ansidecl.h" \
	DEFINES="" \
	/bin/sh ${GCCDIR}/mkconfig.sh ${.TARGET}
.if exists(${GCCDIR}/config/${GCC_CPU}/${GCC_CPU}-modes.def)
	echo '#define EXTRA_MODES_FILE "${GCC_CPU}/${GCC_CPU}-modes.def"' >> ${.TARGET}
.endif

GENSRCS+=	bconfig.h
CLEANFILES+=	cs-bconfig.h

# tconfig.h
tconfig.h:
	TARGET_CPU_DEFAULT="${TARGET_CPU_DEFAULT}" \
	HEADERS="auto-host.h ansidecl.h" \
	DEFINES="USED_FOR_TARGET" \
	/bin/sh ${GCCDIR}/mkconfig.sh ${.TARGET}

GENSRCS+=	tconfig.h
CLEANFILES+=	cs-tconfig.h
# Options
optionlist: ${OPT_FILES}
	awk -f ${GCCDIR}/opt-gather.awk ${.ALLSRC} > ${.TARGET}

options.h:	optionlist
	awk -f ${GCCDIR}/opt-functions.awk \
	    -f ${GCCDIR}/opth-gen.awk \
		< ${.ALLSRC} > ${.TARGET}

options.c:	optionlist
	awk -f ${GCCDIR}/opt-functions.awk \
	    -f ${GCCDIR}/optc-gen.awk \
		-v header_name="config.h system.h coretypes.h tm.h" \
		< ${.ALLSRC} > ${.TARGET}
GENONLY+=	optionlist options.h options.c

# Target machine config
tm.h:
	TARGET_CPU_DEFAULT="${TARGET_CPU_DEFAULT}" \
	HEADERS="${TARGET_INC}" \
	DEFINES="${TM_DEFINES}" \
	/bin/sh ${GCCDIR}/mkconfig.sh ${.TARGET}
.if exists(${GCCDIR}/config/${GCC_CPU}/${GCC_CPU}-modes.def)
	echo '#define EXTRA_MODES_FILE "${GCC_CPU}/${GCC_CPU}-modes.def"' >> ${.TARGET}

# XXX This is an ugly hack to make it search in our old dir too.
	echo '#define GPLUSPLUS_TOOL_INCLUDE_DIR PREFIX"/include/g++/${GCC_TARGET}"' >> ${.TARGET}
.endif

GENSRCS+=	tm.h
CLEANFILES+=	cs-tm.h

# Target machine protos/preds.
tm_p.h:
	TARGET_CPU_DEFAULT="${TARGET_CPU_DEFAULT}" \
	HEADERS="${GCC_CPU}/${GCC_CPU}-protos.h tm-preds.h" \
	DEFINES="" \
	/bin/sh ${GCCDIR}/mkconfig.sh tm_p.h

GENSRCS+=	tm_p.h
CLEANFILES+=	cs-tm_p.h

# gencheck
gencheck.h: ${TREE_DEF_FILES}
.for F in ${TREE_DEF_FILES}
	echo "#include \"$F\""					>> ${.TARGET}
.endfor
	touch ${.TARGET}

GENSRCS+=	gencheck.h


# Source header for gtyp generator.
gtyp-gen.h:	${GTFILES}
	echo "/* This file is machine generated.  Do not edit.  */" > ${.TARGET}
	echo "static const char * const srcdir = "		>> ${.TARGET}
	echo "\"$(GTFILES_SRCDIR)\";"				>> ${.TARGET}
	echo "static const char * const lang_files[] = {"	>> ${.TARGET}
.for F in ${GTFILES_FILES}
	echo "\"$F\", "						>> ${.TARGET}
.endfor
	echo "NULL};"						>> ${.TARGET}
	echo "static const char * const langs_for_lang_files[] = {">> ${.TARGET}
.for F in ${GTFILES_LANGS}
	echo "\"$F\", "						>> ${.TARGET}
.endfor
	echo "NULL};"						>> ${.TARGET}
	echo "static const char * const all_files[] = {"	>> ${.TARGET}
.for F in ${GTFILES}
	echo "\"$F\", "						>> ${.TARGET}
.endfor
	echo "NULL};"						>> ${.TARGET}
	echo "static const char * const lang_dir_names[] = {"	>> ${.TARGET}
.for F in c ${GTFILES_LANG_DIR_NAMES}
	echo "\"$F\", "						>> ${.TARGET}
.endfor
	echo "NULL};"						>> ${.TARGET}

GENSRCS+=	gtyp-gen.h

# Version header for gcov
gcov-iov.h:
	echo "#define GCOV_VERSION ((gcov_unsigned_t)0x34303270)" >> ${.TARGET}

GENSRCS+=	gcov-iov.h

# Multilib config file
multilib.h:
	echo 'static const char *const multilib_raw[] = { \
	    ". ;", NULL };'					> ${.TARGET}
	echo 'static const char *const multilib_matches_raw[] = { \
	    NULL };'						>> ${.TARGET}
	echo 'static const char *multilib_extra = "";'		>> ${.TARGET}
	echo 'static const char *multilib_options = "";'	>> ${.TARGET}
	echo 'static const char *const multilib_exclusions_raw[] = { \
	    NULL };'						>> ${.TARGET}

GENSRCS+=	multilib.h

configargs.h:
	echo 'static const char configuration_arguments[] ='	> ${.TARGET}
	echo '	"OpenBSD/${TARGET_ARCH} system compiler";'	>> ${.TARGET}
	echo 'static const char thread_model[] = "posix";'	>> ${.TARGET}
	echo 'static const struct {'				>> ${.TARGET}
	echo '	const char *name, *value;'			>> ${.TARGET}
	echo '} configure_default_options[] = {'		>> ${.TARGET}
	echo '	{ "NULL", "NULL" } };'				>> ${.TARGET}

GENSRCS+=	configargs.h

# Language spec files
specs.h:
	echo '#include "cp/lang-specs.h"'			> ${.TARGET}
	echo '#include "objc/lang-specs.h"'			>> ${.TARGET}

GENSRCS+=	specs.h

gstdint.h:
	echo '#include "sys/types.h"'				> ${.TARGET}
	echo '#include "sys/stdint.h"'				>> ${.TARGET}

GENSRCS+=	gstdint.h

# Linked headers
gthr-default.h: ${GCCDIR}/gthr-posix.h
	ln -sf ${.ALLSRC} ${.TARGET}

GENSRCS+=	gthr-default.h

.if ${TARGET_ARCH} == "arm"
unwind.h: ${GCCDIR}/config/arm/unwind-arm.h
	ln -sf ${.ALLSRC} ${.TARGET}
.else
unwind.h: ${GCCDIR}/unwind-generic.h
	ln -sf ${.ALLSRC} ${.TARGET}
.endif

GENSRCS+=	unwind.h

#
# gtype gunk
#
gengtype-lex.c:	gengtype-lex.l
	flex -ogengtype-lex.c ${.ALLSRC}

gengtype-yacc.h gengtype-yacc.c: gengtype-yacc.y
	yacc -d -o gengtype-yacc.c ${.ALLSRC}

gengtype-yacc+%DIKED.c: gengtype-yacc.c
	cat    ${.ALLSRC} > ${.TARGET}
	sed -e "s/xmalloc/malloc/g" \
	    -e "s/xrealloc/realloc/g" \
	    -e "s/malloc/xmalloc/g" \
	    -e "s/realloc/xrealloc/g" \
	    ${.ALLSRC} > ${.TARGET}

GENSRCS+= gengtype-lex.c gengtype-yacc.h gengtype-yacc+%DIKED.c
CLEANFILES+= gengtype-yacc.c gengtype-lex.d gengtype-yacc+%DIKED.d

gengtype: gengtype.o gengtype-yacc+%DIKED.o gengtype-lex.o errors.o \
	  ${LIBIBERTY}
	${CC} ${CFLAGS} ${LDFLAGS} -o ${.TARGET} ${.ALLSRC}
CLEANFILES+= gengtype.d

gtype-desc.h:	gengtype
	./gengtype
	touch ${.TARGET}

gtype-desc.c:	gtype-desc.h

GENONLY+=	gtype-desc.c gtype-desc.h
CLEANFILES+=	gt-*.h gtype-*.h

#
# Generator tools.
#
.for F in check checksum genrtl modes
gen$F:	gen$F.o errors.o ${LIBIBERTY}
	${CC} ${CFLAGS} ${LDFLAGS} -o ${.TARGET} ${.ALLSRC}
CLEANFILES+=	gen$F.d
.endfor
CLEANFILES+=	errors.d

.for F in attr attrtab automata codes conditions config constants emit \
	extract flags  opinit output peep preds recog
gen$F:	gen$F.o rtl.o read-rtl.o ggc-none.o vec.o min-insn-modes.o \
	gensupport.o print-rtl.o errors.o ${LIBIBERTY}
	${CC} ${CFLAGS} ${LDFLAGS} -o ${.TARGET} ${.ALLSRC} -lm
CLEANFILES+=	gen$F.d
.endfor
CLEANFILES+=	rtl.d read-rtl.d ggc-none.d vec.d min-insn-modes.d \
	gensupport.d print-rtl.d

gencondmd:	gencondmd.o
	${CC} ${CFLAGS} ${LDFLAGS} -o ${.TARGET} ${.ALLSRC}
CLEANFILES+=	gencondmd.d

#
# Generated .md files.
#
insn-conditions.md:	gencondmd
	./gencondmd > ${.TARGET}
GENSRCS+=	insn-conditions.md

#
# Generated header files.
#

.for F in constants
insn-$F.h:	gen$F ${MD_FILE}
	./gen$F ${MD_FILE} > ${.TARGET}
GENSRCS+=	insn-$F.h
.endfor

.for F in attr codes config flags
insn-$F.h:	gen$F ${MD_FILE} insn-conditions.md
	./gen$F ${MD_FILE} insn-conditions.md > ${.TARGET}
GENSRCS+=	insn-$F.h
.endfor

# Header files with irregular names.
genrtl.h:	gengenrtl
	./gengenrtl -h > ${.TARGET}
GENSRCS+=	genrtl.h

tm-preds.h:	genpreds
	./genpreds -h ${MD_FILE} > ${.TARGET}
GENSRCS+=	tm-preds.h

tm-constrs.h:	genpreds
	./genpreds -c ${MD_FILE} > ${.TARGET}
GENSRCS+=	tm-constrs.h

tree-check.h:	gencheck
	./gencheck > ${.TARGET}
GENSRCS+=	tree-check.h

insn-modes.h:	genmodes
	./genmodes -h > ${.TARGET}
GENSRCS+=	insn-modes.h

#
# Generated source files.
#
.for F in attrtab automata emit extract opinit output peep preds recog
insn-$F.c:	gen$F ${MD_FILE} insn-conditions.md
	./gen$F ${MD_FILE} insn-conditions.md > ${.TARGET}
GENONLY+=	insn-$F.c
.endfor

.for F in conditions
insn-$F.c:	gen$F ${MD_FILE}
	./gen$F ${MD_FILE} > ${.TARGET}
GENSRCS+=	insn-$F.c
.endfor

# Source files with irregular names.
insn-modes.c:	genmodes
	./genmodes > ${.TARGET}
GENONLY+=	insn-modes.c

min-insn-modes.c:	genmodes
	./genmodes -m > ${.TARGET}
GENSRCS+=	min-insn-modes.c

genrtl.c:	gengenrtl
	./gengenrtl > ${.TARGET}
GENONLY+=	genrtl.c

gencondmd.c:	genconditions ${MD_FILE}
	./genconditions ${MD_FILE} > ${.TARGET}
GENSRCS+=	gencondmd.c

#-----------------------------------------------------------------------
# Build tools.

GNTOOLS+=	genattr genattrtab genautomata gencodes gencheck genchecksum \
		genconditions gencondmd genconfig genconstants genemit \
		genextract genflags gengenrtl gengtype genmodes genopinit \
		genoutput genpeep genpreds genrecog

all: ${GNTOOLS} ${GENSRCS} ${GENONLY}

#-----------------------------------------------------------------------
# Fixups.

# Set OBJS the same as bsd.prog.mk would do if we defined PROG.  We can't
# define PROG because we have multiple programs.
#
SRCS=		errors.c genattr.c genattrtab.c \
		genautomata.c gencheck.c genchecksum.c gencodes.c \
		genconditions.c genconfig.c genconstants.c genemit.c \
		genextract.c genflags.c gengenrtl.c gengtype.c genmodes.c \
		genopinit.c genoutput.c genpeep.c genpreds.c genrecog.c \
		gensupport.c ggc-none.c print-rtl.c read-rtl.c rtl.c \
		vec.c

SRCS+=		${GENSRCS}
OBJS+=		${SRCS:N*.h:R:S/$/.o/g}
GENOBJS+=	${GENSRCS:N*.h:R:S/$/.o/g}
CLEANFILES+=	${GENSRCS} ${GENONLY} ${GENOBJS} ${GNTOOLS}

#-----------------------------------------------------------------------
# Manual dependencies.
.include  "Makefile.dep"

.include <bsd.prog.mk>
# DO NOT DELETE
