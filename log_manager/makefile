include ../build_gateway.inc
include ../makefile.inc

CC = gcc
CPP = g++

SRCS = log_manager.cc

LOG_WRITER_PATH := $(shell pwd)

makeall: clean all

clean:
	make -C ../utils clean
	- $(DEL) *.o 
	- $(DEL) *.so
	- $(DEL) *.so.1.0.1 
	- $(DEL) *~
	- $(DEL) depend

all: utils lib

utils:
	make -C $(ROOT_PATH)/utils clean all
	$(COPY) $(ROOT_PATH)/utils/skygw_utils.o ./

lib: libcomp liblink

libcomp: 
	$(CPP) -c $(CFLAGS) \
	$(MYSQL_HEADERS) \
	-I../utils/ -I./ \
	-fPIC ./log_manager.cc -o log_manager.o $(LDLIBS)

liblink:
	 $(CPP) -shared \
	-Wl,-soname,liblog_manager.so \
	-o liblog_manager.so.1.0.1 log_manager.o \
	$(LDLIBS) $(CPP_LDLIBS)
	$(DEL) ./liblog_manager.so
	$(LINK) ./liblog_manager.so.1.0.1 ./liblog_manager.so

install: liblink
	install liblog_manager.so.1.0.1 liblog_manager.so $(DEST)/lib

depend:
	@rm -f depend
	$(CPP) -M $(CFLAGS) \
	$(MYSQL_HEADERS) \
	-I../utils/ -I./ \
	$(SRCS) > depend

include depend
