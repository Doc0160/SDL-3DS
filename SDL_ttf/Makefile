ifeq ($(strip $(DEVKITARM)),)
$(error "Please set DEVKITARM in your environment. export DEVKITARM=<path to>devkitARM")
endif

include $(DEVKITARM)/base_tools

TARGET_LIB = libSDL_ttf.a

OBJS =	SDL_ttf.o 

CTRULIB	:= $(DEVKITPRO)/libctru
INCLUDES = -I./include -I$(CTRULIB)/include 

#---------------------------------------------------------------------------------
# options for code generation
#---------------------------------------------------------------------------------
ARCH		:=	-march=armv6k -mtune=mpcore -mfloat-abi=hard
CFLAGS		:=	-g -Wall -O2 -mword-relocations \
			-fomit-frame-pointer -ffast-math \
			$(ARCH)
CFLAGS		+=	$(INCLUDES) -DARM11 -D_3DS -D__3DS__ -DENABLE_3DS -DSDL_LOADSO_DUMMY \
				-I$(CTRULIB)/../portlibs/armv6k/include -I$(CTRULIB)/../portlibs/3ds/include/SDL \
				-I$(CTRULIB)/../portlibs/armv6k/include/freetype2
CXXFLAGS	:=      $(CFLAGS) -fno-rtti -fno-exceptions -std=gnu++11
ASFLAGS		:=	-g $(ARCH)
LDFLAGS		:=	-specs=3dsx.specs -g $(ARCH) -Wl,-Map,$(notdir $*.map)

CFLAGS		+= -Wno-unused-variable

LIBS	:= -freetype -lSDL -lctru -lm

all: $(OBJS) $(TARGET_LIB)

$(TARGET_LIB): $(OBJS)
	$(AR) -rc $@ $^

install: $(TARGET_LIB)
	@mkdir -p "$(DEVKITPRO)/portlibs/3ds/lib"
	@cp  $(TARGET_LIB) "$(DEVKITPRO)/portlibs/3ds/lib"
	@mkdir -p "$(DEVKITPRO)/portlibs/3ds/include"
	@mkdir -p "$(DEVKITPRO)/portlibs/3ds/include/SDL"
	@cp SDL_ttf.h "$(DEVKITPRO)/portlibs/3ds/include/SDL"

	@echo "Installed!"

clean:
	@rm -rf $(OBJS) $(TARGET_LIB)