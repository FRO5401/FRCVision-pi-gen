IMG_VERSION?=$(shell git describe)
DEPS_CFLAGS?=$(shell pkg-config --cflags cscore wpiutil)
DEPS_LIBS?=$(shell pkg-config --libs --static cscore wpiutil)
FRC_JSON?=/boot/frc.json
GST_JSON?=/boot/gst.json
DHCPCD_CONF?=/boot/dhcpcd.conf
APP_UID?=1000
APP_GID?=1000

.PHONY: all clean
.SUFFIXES:

all: configServer

clean:
	rm -f configServer
	rm -f src/*.o
	rm -f src/resources/*.o

OBJS= \
    src/main.o \
    src/Application.o \
    src/MyHttpConnection.o \
    src/NetworkSettings.o \
    src/SystemStatus.o \
    src/VisionSettings.o \
    src/VisionStatus.o \
    src/WebSocketHandlers.o \
    src/resources/index.html.o \
    src/resources/frcvision.css.o \
    src/resources/frcvision.js.o \
    src/resources/froicon.png.o

configServer: ${OBJS}
	${CXX} -pthread -g -o $@ ${CXXFLAGS} $^ ${DEPS_LIBS}

%.o: %.cpp
	${CXX} -g -O -Wall -c -o $@ \
	    ${CXXFLAGS} \
	    ${DEPS_CFLAGS} \
	    '-DEXEC_HOME="/home/${FIRST_USER_NAME}"' \
	    '-DFRC_JSON="${FRC_JSON}"' \
	    '-DGST_JSON="${GST_JSON}"' \
	    '-DDHCPCD_CONF="${DHCPCD_CONF}"' \
	    '-DAPP_UID=${APP_UID}' \
	    '-DAPP_GID=${APP_GID}' \
	    $<

%.html.cpp: %.html
	env IMG_VERSION=${IMG_VERSION} ./gen_resource.py $@ $<

%.css.cpp: %.css
	env IMG_VERSION=${IMG_VERSION} ./gen_resource.py $@ $<

%.js.cpp: %.js
	env IMG_VERSION=${IMG_VERSION} ./gen_resource.py $@ $<

%.png.cpp: %.png
	env IMG_VERSION=${IMG_VERSION} ./gen_resource.py $@ $<

