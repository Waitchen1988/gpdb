#-------------------------------------------------------------------------
#
# Makefile--
#    Makefile for the Wisconsin Benchmark
#
# Portions Copyright (c) 1996-2009, PostgreSQL Global Development Group
# Portions Copyright (c) 1994-5, Regents of the University of California
#
#
# IDENTIFICATION
#    $PostgreSQL: pgsql/src/test/bench/Makefile,v 1.16 2008/03/18 16:24:50 petere Exp $
#
#-------------------------------------------------------------------------

subdir = src/test/bench
top_builddir = ../../..
include $(top_builddir)/src/Makefile.global

CREATEFILES= create.sql bench.sql
OUTFILES= bench.out bench.out.perquery


all: $(CREATEFILES)
	rm -f $(OUTFILES)

create.sql: create.source
	rm -f $@; \
	C=`pwd`; \
	sed -e "s:_CWD_:$$C:g"  < $< > $@

bench.sql: 
	x=1; \
	for i in `ls query[0-9][0-9]`; do \
	  echo "select $$x as x" >> bench.sql && \
	  cat $$i >> bench.sql && \
	  x=`expr $$x + 1` || exit; \
	done

runtest: $(OUTFILES)

bench.out: $(CREATEFILES)
	$(SHELL) ./create.sh $$PGDATA && \
	$(SHELL) ./runwisc.sh $$PGDATA > $@
	@echo "RESULTS OF BENCHMARK ARE SAVED IN FILE bench.out";

bench.out.perquery: bench.out
	$(SHELL) ./perquery <bench.out > $@
	@echo "BREAKDOWN OF BENCHMARK IS SAVED IN FILE bench.out.perquery";

clean:
	rm -f $(OUTFILES) $(CREATEFILES)
