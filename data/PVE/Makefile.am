bin_SCRIPTS = pvecm

IPCC.c: IPCC.xs
	xsubpp IPCC.xs > IPCC.xsc
	mv IPCC.xsc IPCC.c

CFLAGS = -shared -fPIC -Wall -I/usr/lib/perl/5.14.2/CORE -DXS_VERSION=VERSION
LDADD = $(QB_LIBS)

perlbin_PROGRAMS = IPCC.so
perlbindir = /usr/lib/perl5/auto/PVE/IPCC/

%.1.gz: %.1.pod
	cat $<|pod2man -n $* -s 1 -r ${VERSION} -c "Proxmox Documentation"|gzip -c9 >$@

%.5.gz: %.5.pod
	cat $<|pod2man -n $* -s 5 -r ${VERSION} -c "Proxmox Documentation"|gzip -c9 >$@

%.1.pod: %
	perl -I.. ./$* printmanpod > $@

datacenter.cfg.5.pod: Cluster.pm gen-datacenter-pod.pl
	perl -I.. ./gen-datacenter-pod.pl > $@

#datacenter.cfg.5.gz: Cluster.pm gen-datacenter-pod.pl
#	./gen-datacenter-pod.pl|pod2man -n datacenter.cfg -s 5 -r ${VERSION} -c #"Proxmox Documentation"|gzip -c9 >$@


pod_DATA = pvecm.1.pod datacenter.cfg.5.pod
poddir = /usr/share/doc/pve-cluster/pod

man1_MANS = pvecm.1.gz
man5_MANS = datacenter.cfg.5.gz

IPCC_so_SOURCES = IPCC.c ppport.h

pvelib_DATA = IPCC.pm Cluster.pm
pvelibdir = /usr/share/perl5/PVE

install-exec-hook:
	perl -I.. ./pvecm verifyapi

CLEANFILES = IPCC.c *~ *.1.gz *.5.gz *.pod
