include Makefile.include

CFLAGS+= -std=c++0x -D__STDC_CONSTANT_MACROS -D__STDC_LIMIT_MACROS -DTARGET_POSIX -DTARGET_LINUX -fPIC -DPIC -D_REENTRANT -D_LARGEFILE64_SOURCE -D_FILE_OFFSET_BITS=64 -DHAVE_CMAKE_CONFIG -D__VIDEOCORE4__ -U_FORTIFY_SOURCE -Wall -DHAVE_OMXLIB -DUSE_EXTERNAL_FFMPEG  -DHAVE_LIBAVCODEC_AVCODEC_H -DHAVE_LIBAVUTIL_OPT_H -DHAVE_LIBAVUTIL_MEM_H -DHAVE_LIBAVUTIL_AVUTIL_H -DHAVE_LIBAVFORMAT_AVFORMAT_H -DHAVE_LIBAVFILTER_AVFILTER_H -DHAVE_LIBSWRESAMPLE_SWRESAMPLE_H -DOMX -DOMX_SKIP64BIT -ftree-vectorize -DUSE_EXTERNAL_OMX -DTARGET_RASPBERRY_PI -DUSE_EXTERNAL_LIBBCM_HOST

LDFLAGS+=-L./ -ldbus-1 -lWFC -lGLESv2 -lEGL -lbcm_host -lopenmaxil -lfreetype -lz
INCLUDES+=-I./ -Ilinux

SRC=linux/XMemUtils.cpp \
    utils/log.cpp \
    DynamicDll.cpp \
    utils/PCMRemap.cpp \
    utils/RegExp.cpp \
    OMXSubtitleTagSami.cpp \
    OMXOverlayCodecText.cpp \
    BitstreamConverter.cpp \
    linux/RBP.cpp \
    OMXThread.cpp \
    OMXReader.cpp \
    OMXStreamInfo.cpp \
    OMXAudioCodecOMX.cpp \
    OMXCore.cpp \
    OMXVideo.cpp \
    OMXAudio.cpp \
    OMXClock.cpp \
    File.cpp \
    OMXPlayerVideo.cpp \
    OMXPlayerAudio.cpp \
    OMXPlayerSubtitles.cpp \
    SubtitleRenderer.cpp \
    Unicode.cpp \
    Srt.cpp \
    KeyConfig.cpp \
    OMXControl.cpp \
    Keyboard.cpp \
    omxplayer.cpp \

OBJS+=$(filter %.o,$(SRC:.cpp=.o))

all: omxplayer.bin

%.o: %.cpp
	@rm -f $@ 
	$(CXX) $(CFLAGS) $(INCLUDES) -c $< -o $@ -Wno-deprecated-declarations

list_test:
	$(CXX) -O3 -o list_test list_test.cpp

version:
	bash gen_version.sh > version.h

omxplayer.bin: version $(OBJS)
	$(CXX) $(LDFLAGS) -o omxplayer.bin $(OBJS) -lvchiq_arm -lvcos -lrt -lpthread -lavutil -lavcodec -lavformat -lswscale -lswresample -lpcre

clean:
	for i in $(OBJS); do (if test -e "$$i"; then ( rm $$i ); fi ); done
	@rm -f omxplayer.old.log omxplayer.log
	@rm -f omxplayer.bin
	@rm -rf omxplayer-dist
	@rm -f omxplayer-dist.tar.gz

install:
	install -d $(DESTDIR)/usr/bin
	install -Dm755 omxplayer $(DESTDIR)/usr/bin/omxplayer
	install -Dm755 omxplayer.bin $(DESTDIR)/usr/bin

	
