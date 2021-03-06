# For multiple programs using a single source file each,
# we can just define 'progs' and create custom targets.
PROGS	=	pkt-gen bridge testpcap libnetmap.so
#PROGS += pingd
PROGS	+= testlock testcsum test_select testmmap vale-ctl
MORE_PROGS = kern_test

CLEANFILES = $(PROGS) pcap.o nm_util.o *.o
NO_MAN=
CFLAGS = -O2 -pipe
CFLAGS += -Werror -Wall -Wunused-function
CFLAGS += -I ../sys # -I/home/luigi/FreeBSD/head/sys -I../sys
CFLAGS += -Wextra
.ifdef WITH_PCAP
LDFLAGS += -lpcap
.else
CFLAGS += -DNO_PCAP
.endif

LDFLAGS += -lpthread
LDFLAGS += -lrt	# needed on linux, does not harm on BSD
#SRCS = pkt-gen.c

all: $(PROGS)

testpcap: pcap.c libnetmap.so nm_util.o
	$(CC) $(CFLAGS) -DTEST -o testpcap pcap.c nm_util.o $(LDFLAGS) -lpcap

kern_test: testmod/kern_test.c

nm_util.o pkt-gen.o bridge.o libnetmap.so pcap.o: nm_util.h

pkt-gen: pkt-gen.o nm_util.o
	$(CC) $(CFLAGS) -o pkt-gen pkt-gen.o nm_util.o $(LDFLAGS)

bridge: bridge.o nm_util.o
	$(CC) $(CFLAGS) -o bridge bridge.o nm_util.o

vale-ctl: vale-ctl.o
	$(CC) $(CFLAGS) -o vale-ctl vale-ctl.o

libnetmap.so:	pcap.c nm_util.c
	$(CC) $(CFLAGS) -fpic -c pcap.c
	$(CC) $(CFLAGS) -fpic -c nm_util.c
	$(CC) -shared -o libnetmap.so pcap.o nm_util.o

clean:
	-@rm -rf $(CLEANFILES)

testlock: testlock.c
	$(CC) $(CFLAGS) -o testlock testlock.c -lpthread $(LDFLAGS)
