GEN_CUSTOM_HEADER:=../Common/generated_custom.h

include custom.makefile 
include config.makefile

GNUTOOLS_PATH=/usr/local/$(GNUTOOLS_VERSION)/bin

.PHONY: build_libs $(MAKECMDGOALS)

	
all: $(GEN_CUSTOM_HEADER) build_libs $(TO_BUILD)
$(GEN_CUSTOM_HEADER): custom.makefile
	@echo "#ifndef _GENERATED_CUSTOM_CONFIGURATION_H_" > $@
	@echo "#define _GENERATED_CUSTOM_CONFIGURATION_H_" >> $@
	@echo >> $@
	@echo "#if  defined(BR2_PACKAGE_BCM4318_AP)" >> $@
	@echo "#  define AP" >> $@
	@echo "#else" >> $@
	@echo "#  define STA" >> $@
	@echo "#endif" >> $@
	@echo "#define CURRENT_NUM_VERSION_SOFT \"$(MAJOR_VERSION).$(MINOR_VERSION).$(MODIF_VERSION)\"" >> $@
	@echo "#define CURRENT_BUILD_DATE \"$(shell date +%F\ %H:%M)\"" >> $@
	@echo >> $@
ifeq ("$(VIDEO_YUV)","yes")
	@echo "#define USE_VIDEO_YUV" >> $@
endif
ifeq ("$(RECORD_VISION_DATA)","yes")
	@echo "#define RECORD_VISION_DATA" >> $@
endif
	@echo >> $@
	@echo "#define WIFI_NETWORK_NAME \"$(WIFI_NETWORK_NAME)\"" >> $@
	@echo "#define WIFI_BROADCAST \"$(WIFI_BROADCAST)\"" >> $@
	@echo "#define WIFI_ARDRONE_IP \"$(WIFI_ARDRONE_IP)\"" >> $@
	@echo >> $@
	@echo "#if defined(__linux__) || defined(USE_MINGW32)" >> $@
	@echo "# define WIFI_MOBILE_IP \"$(WIFI_MOBILE_IP)\"" >> $@
	@echo "# define WIRED_ITFNAME \"$(WIRED_ITFNAME)\"" >> $@
	@echo "#endif // ! __linux__" >> $@
	@echo >> $@
	@echo >> $@
	@echo "#endif // ! _GENERATED_CUSTOM_CONFIGURATION_H_" >> $@

ifneq "$(MAKECMDGOALS)" ""
  ifneq "$(MAKECMDGOALS)" "clean"
    ifneq "$(MAKECMDGOALS)" "update"
      $(MAKECMDGOALS):
      @echo -e "\nCannot make what you ask me to do :-("
    else
      @echo -e "\nCannot support"
    endif
  endif
endif

$(MAKECMDGOALS): build_libs

checkpackages:
	sh $(shell pwd)/check_dependencies.sh static RELEASE_BUILD=$(RELEASE_BUILD) $(MAKECMDGOALS)

define GENERIC_RULES_TEMPLATE
build_$(1): 
	@$(MAKE) -C $(1) $(MAKECMDGOALS)
endef

$(foreach target,$(TARGETS),$(eval $(call GENERIC_RULES_TEMPLATE,$(target))))

build_libs: checkpackages
	@$(MAKE) PC_TARGET=yes USE_ARDRONE_TOOL=yes TARGET=pc_ USE_MINGW32=no -C ../Lib/Build $(MAKECMDGOALS)
	@$(MAKE) PC_TARGET=yes USE_ARDRONE_TOOL=no TARGET=pc_ USE_MINGW32=no -C ../Lib/Build $(MAKECMDGOALS)
	
