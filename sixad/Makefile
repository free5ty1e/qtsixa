# You know, there are pre-compile DEBs of this...

CXX ?= g++
CXXFLAGS ?= -O2 -Wall
LDFLAGS += -Wl,-Bsymbolic-functions

GASIA_GAMEPAD_HACKS = true # Set to 'true' to enable hacks
SHANWAN_FAKE_DS3 = true #set to 'true' to enable hacks

ifeq ($(GASIA_GAMEPAD_HACKS),true)
CXXFLAGS += -DGASIA_GAMEPAD_HACKS
endif

ifdef SHANWAN_FAKE_DS3
	CXXFLAGS += -DSHANWAN_FAKE_DS3
endif

all: sixad_bins

sixad_bins:
	mkdir -p bins
	$(CXX) $(CXXFLAGS) $(LDFLAGS) sixad-bin.cpp bluetooth.cpp shared.cpp textfile.cpp -o bins/sixad-bin `pkg-config --cflags --libs bluez` -lpthread -fpermissive
	$(CXX) $(CXXFLAGS) $(LDFLAGS) sixad-sixaxis.cpp sixaxis.cpp shared.cpp uinput.cpp textfile.cpp -o bins/sixad-sixaxis -lpthread -lrt
	$(CXX) $(CXXFLAGS) $(LDFLAGS) sixad-remote.cpp remote.cpp shared.cpp uinput.cpp textfile.cpp -o bins/sixad-remote -lrt
	$(CXX) $(CXXFLAGS) $(LDFLAGS) sixad-raw.cpp sixaxis.cpp shared.cpp uinput.cpp textfile.cpp -o bins/sixad-raw
	$(CXX) $(CXXFLAGS) $(LDFLAGS) sixad-3in1.cpp sixaxis.cpp shared.cpp uinput.cpp textfile.cpp -o bins/sixad-3in1

clean:
	rm -f *~ bins/*

install:
	install -d $(DESTDIR)/etc/default/
	install -d $(DESTDIR)/etc/init.d/
	install -d $(DESTDIR)/etc/logrotate.d/
	install -d $(DESTDIR)/usr/bin/
	install -d $(DESTDIR)/usr/sbin/
	install -d $(DESTDIR)/var/lib/sixad/
	install -d $(DESTDIR)/var/lib/sixad/profiles/
	install -m 644 sixad.default $(DESTDIR)/etc/default/sixad
	install -m 755 sixad.init $(DESTDIR)/etc/init.d/sixad
	install -m 644 sixad.log $(DESTDIR)/etc/logrotate.d/sixad
	install -m 755 sixad $(DESTDIR)/usr/bin/
	install -m 755 bins/sixad-bin $(DESTDIR)/usr/sbin/
	install -m 755 bins/sixad-sixaxis $(DESTDIR)/usr/sbin/
	install -m 755 bins/sixad-remote $(DESTDIR)/usr/sbin/
	install -m 755 bins/sixad-3in1 $(DESTDIR)/usr/sbin/
	install -m 755 bins/sixad-raw $(DESTDIR)/usr/sbin/
	@chmod 777 -R $(DESTDIR)/var/lib/sixad/
	@echo "Installation is Complete!"

uninstall:
	rm -f $(DESTDIR)/etc/default/sixad
	rm -f $(DESTDIR)/etc/init.d/sixad
	rm -f $(DESTDIR)/etc/logrotate.d/sixad
	rm -f $(DESTDIR)/usr/bin/sixad
	rm -f $(DESTDIR)/usr/sbin/sixad-bin
	rm -f $(DESTDIR)/usr/sbin/sixad-sixaxis
	rm -f $(DESTDIR)/usr/sbin/sixad-remote
	rm -f $(DESTDIR)/usr/sbin/sixad-raw
	rm -rf $(DESTDIR)/var/lib/sixad/
