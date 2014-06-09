# Network utilities for arduino

This block contains at the moment:
1. An Arduino SDK compatible client for Adafruit CC3000 wifi (i.e. inheriting from WiFiClient)
2. Simple http_get methods that accept both a generic SDK Client and a (non SDK compatible) Adafruit_CC3000_Client
3. Some CC3000 related functions, commonly repeated in examples. startConnection, listSSIDResult, displayConnectionDetails

Following there is an example of how you can use it:



	#include "Arduino.h"

	#include <adafruit/cc3000_library/adafruit_cc3000.h>
	#include "diego/ardunet/cc3000utils.h"
	#include "diego/ardunet/cc3000client.h"
	#include "diego/ardunet/http_get.h"
	#include "../../../mywifidetails.h"
	//This file, located in your hive root folder must #define WLAN_SSID , WLAN_PASS and WLAN_SECURITY 
	//of your wifi. It is not published in your block for obvious reasons. Example:
	//define WLAN_SSID       "YOUR_SSID"           // cannot be longer than 32 characters!
	//#define WLAN_PASS       "your_password"
	// Security can be WLAN_SEC_UNSEC, WLAN_SEC_WEP, WLAN_SEC_WPA or WLAN_SEC_WPA2
	//#define WLAN_SECURITY   YOUR_WIFI_SECURITY

	// These are the interrupt and control pins
	#define ADAFRUIT_CC3000_IRQ   3  // MUST be an interrupt pin!
	// These can be any two pins
	#define ADAFRUIT_CC3000_VBAT  5
	#define ADAFRUIT_CC3000_CS    10


	//create CC3000 instance, and an SDK compatible Client
	Adafruit_CC3000 cc3000 = Adafruit_CC3000(ADAFRUIT_CC3000_CS,
			ADAFRUIT_CC3000_IRQ, ADAFRUIT_CC3000_VBAT, SPI_CLOCK_DIVIDER); 

	CC3000Client client(cc3000);

	void setup(void) {
		Serial.begin(9600);
		startConnection(cc3000, WLAN_SSID, WLAN_PASS, WLAN_SECURITY);
	}

	void loop(void) {
		client.connect("www.google.com", 80);
		http_get(client, "www.google.com", "/");
		client.stop();
		delay(10000);
	}