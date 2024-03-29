DEVELHELP := 1
# name of your application
include ../Makefile.tests_common

BOARD_INSUFFICIENT_MEMORY := arduino-duemilanove arduino-leonardo \
                             arduino-mega2560 arduino-nano arduino-uno hifive1 \
                             hifive1b i-nucleo-lrwan1 mega-xplained msb-430 \
                             msb-430h nucleo-f030r8 nucleo-f031k6 \
                             nucleo-f042k6 nucleo-f070rb nucleo-f072rb \
                             nucleo-f303k8 nucleo-f334r8 nucleo-l031k6 \
                             nucleo-l053r8 saml10-xpro saml11-xpro \
                             stm32f0discovery stm32l0538-disco telosb \
                             waspmote-pro wsn430-v1_3b wsn430-v1_4 z1
# chronos, hamilton, ruuvitag, and thingy52 boards don't support ethos
BOARD_BLACKLIST := chronos hamilton ruuvitag thingy52

export TAP ?= tap0

CFLAGS += -DOUTPUT=TEXT
CFLAGS += -DTEST_SUITES="gnrc_ipv6_ext_frag"
CFLAGS += -DGNRC_IPV6_EXT_FRAG_LIMITS_POOL_SIZE=3

ifeq (native,$(BOARD))
  USEMODULE += netdev_tap
  TERMFLAGS ?= $(TAP)
else
  USEMODULE += stdio_ethos

  ETHOS_BAUDRATE ?= 115200
  CFLAGS += -DETHOS_BAUDRATE=$(ETHOS_BAUDRATE)
  TERMDEPS += ethos
  TERMPROG ?= sudo $(RIOTTOOLS)/ethos/ethos
  TERMFLAGS ?= $(TAP) $(PORT) $(ETHOS_BAUDRATE)
endif
USEMODULE += auto_init_gnrc_netif
# add dummy interface to test forwarding to smaller MTU
USEMODULE += netdev_test
GNRC_NETIF_NUMOF := 2
# Specify the mandatory networking modules for IPv6
USEMODULE += gnrc_ipv6_router_default
USEMODULE += gnrc_icmpv6_error
USEMODULE += gnrc_pktdump
USEMODULE += gnrc_pktbuf_cmd
# IPv6 extension headers
USEMODULE += gnrc_ipv6_ext_frag
# UDP support for payload
USEMODULE += gnrc_udp
USEMODULE += od
# Add unittest framework
USEMODULE += embunit
# Add also the shell, some shell commands
USEMODULE += shell
USEMODULE += shell_commands
USEMODULE += ps

# The test requires some setup and to be run as root
# So it cannot currently be run
TEST_ON_CI_BLACKLIST += all

.PHONY: ethos

ethos:
	$(Q)env -u CC -u CFLAGS make -C $(RIOTTOOLS)/ethos

include $(RIOTBASE)/Makefile.include
