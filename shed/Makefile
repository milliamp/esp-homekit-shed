PROGRAM = shed

EXTRA_COMPONENTS = \
	extras/dht \
	extras/http-parser \
	extras/dhcpserver \
	extras/rboot-ota \
	$(abspath ../components/wifi_config) \
	$(abspath ../components/wolfssl) \
	$(abspath ../components/cJSON) \
	$(abspath ../components/homekit)

# DHT11 sensor pins
SENSOR1_PIN = 5 # D1
SENSOR2_PIN = 4 # D2
SENSOR3_PIN = 13 # D7

SENSOR1_DHT22=true
SENSOR2_DHT22=true
SENSOR3_DHT22=false

# Relay bank signal pins
RELAY1_PIN = 2 # D4
#RELAY2_PIN = 14 # D5
RELAY3_PIN = 12 # D6
RELAY4_PIN = 15 # D8
RELAY5_PIN = 16 # D0
#RELAY6_PIN =
#RELAY7_PIN = 
#RELAY8_PIN = 

MOTION_SENSOR_PIN = 14 # D5
INTERIOR_LIGHTS = $(RELAY1_PIN)
#PHONE_CHARGER = $(RELAY2_PIN)
EXTERIOR_FLOOD = $(RELAY3_PIN)
EASTWEST_FAN = $(RELAY4_PIN)
WESTEAST_FAN = $(RELAY5_PIN)

BUTTON_PIN = 0 # D3

ESPIP=192.168.0.72
ESPPORT=/dev/cu.wchusbserial1410
ESPBAUD=115200
#ESPBAUD=350877
FLASH_SIZE=4
FLASH_MODE=dio

BUILD_VERSION=`date +'"%Y%m%d.%H%M"'`

#HOMEKIT_DEBUG=1

EXTRA_CFLAGS += -I../.. -DHOMEKIT_SHORT_APPLE_UUIDS \
	-DBUILD_VERSION=$(BUILD_VERSION) \
	-DSENSOR1_PIN=$(SENSOR1_PIN) \
	-DSENSOR2_PIN=$(SENSOR2_PIN) \
	-DSENSOR3_PIN=$(SENSOR3_PIN) \
	-DSENSOR1_DHT22=$(SENSOR1_DHT22) \
	-DSENSOR2_DHT22=$(SENSOR2_DHT22) \
	-DSENSOR3_DHT22=$(SENSOR3_DHT22) \
	-DMOTION_SENSOR_PIN=$(MOTION_SENSOR_PIN) \
	-DINTERIOR_LIGHTS=$(INTERIOR_LIGHTS) \
	-DEXTERIOR_FLOOD=$(EXTERIOR_FLOOD) \
	-DWESTEAST_FAN=$(WESTEAST_FAN) \
	-DEASTWEST_FAN=$(EASTWEST_FAN) \
	-DBUTTON_PIN=$(BUTTON_PIN)

# This is to ensure the datetime is update in firmware revision
all:
	@touch shed.c

include $(SDK_PATH)/common.mk

monitor:
	$(FILTEROUTPUT) --port $(ESPPORT) --baud 115200 --elf $(PROGRAM_OUT)

ota:
	echo verbose > tftp.tftp
	echo binary >> tftp.tftp
	echo put $(FW_FILE) firmware.bin >> tftp.tftp
	echo quit >> tftp.tftp
	tftp $$ESPIP < tftp.tftp || true
	rm tftp.tftp