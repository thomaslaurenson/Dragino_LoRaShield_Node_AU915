# Dragino LoRaShield - Node Configuration for Australian and New Zealand Frequencies (AU915 MHz)

The Dragino LoRaShield is a popular example of an easy to use development/prototype board for use in LoRaWAN networks. The Dragino LoRaShield is an Arduino shield that provides the ability to function as either a LoRaWAN node, or a single channel LoRaWAN gateway. This documentation, and accompanying source code focusses solely on creating a development LoRaWAN node. The specific purpose of this repository (and guide) is to provide an example of the Dragino LoRaShield device operating on the standardised LoRaWAN frequencies for Australia and New Zealand... specifically the 915 MHz to 930 MHz frequency, more commonly known as *AU915*.

## Quick Start

1. Download this repo as a zip file
2. Add library to Arduino IDE using add a zip file functionality
3. Plug in Arduino with a Dragino LoRaShield attached
4. Optional: Edit source code */examples/Dragino_LoRaShield_AU915/Dragino_LoRaShield_AU915.ino* file to send different output/add code for sensors etc.
5. Select Upload to compile, then upload
6. View device is functioning correctly by viewing serial output
7. Done.

## Australian and New Zealand Frequencies

To operate LoRaWAN in Australia and New Zealand, a device should use the radio frequencies specified by the LoRaWAN standard. This allows interoperation as well as operating radio devices within the regulartions specified by the government. The standardised LoRaWAN frequencies for Australia and New Zealand range from 915 MHz to 930 MHz frequency. In the world of LoRaWAN this standardisation is more commonly known as *AU915*. This information is documented in the official frequency plans in the LoRaWAN 1.0.2 Specification. More specifically, a companion document named [LoRaWAN Regional Parameters](https://www.scribd.com/document/337679415/LoRaWAN-Regional-Parameters-v1-0-20161012-1397-1) outlines the frequencies for use in different countries.

AU915 allows for a total of 73 possible channels to use within the 915 MHz to 930 MHz frequency. Most LoRa networks in Australia seem to have made a consensus to only use a specific portion of this, that is, a specific band of channels in the frequency range. The consensus is the use of: channels 8 to 15 on the AU915 specification. This is also known as AU915 sub-band 2. This band in the specification that is implemented in the TTN server source code and is documented and distributed in the [`global_conf.json`](https://github.com/TheThingsNetwork/gateway-conf/blob/master/AU-global_conf.json) gateway configuration file distributed by TTN. Furthermore, the TTN community has specified the AU915 frequencies and channels on their [LoRaWAN Frequencies](https://www.thethingsnetwork.org/wiki/LoRaWAN/Frequencies/Frequency-Plans) page. Therefore, this makes the channel choice logical for this project, as we want a legal and standardised LoRaWAN solution.

The entire point of the task is to get the Dragino LoRaShield to operate in the specified AU915 frequency range, and specifically, sub-band 2. Therefore, it is first prudent to document the required channels and their respective properties. The table below displays the available upstream and downstream channels for AU915 sub-band 2.

| Channel | Direction | Frequency (MHz) | Bandwidth (kHz) |  Data Rate |
|:-------:|:---------:|:---------------:|:---------------:|:----------:|
|    8    |     up    |      916.8      |       125       |  DR0 - DR3 |
|    9    |     up    |      917.0      |       125       |  DR0 - DR3 |
|    10   |     up    |      917.2      |       125       |  DR0 - DR3 |
|    11   |     up    |      917.4      |       125       |  DR0 - DR3 |
|    12   |     up    |      917.6      |       125       |  DR0 - DR3 |
|    13   |     up    |      917.8      |       125       |  DR0 - DR3 |
|    14   |     up    |      918.0      |       125       |  DR0 - DR3 |
|    15   |     up    |      918.2      |       125       |  DR0 - DR3 |
|    65   |     up    |      917.5      |       500       |     DR4    |
|    0    |    down   |      923.3      |       500       | DR8 - DR13 |
|    1    |    down   |      923.9      |       500       | DR8 - DR13 |
|    2    |    down   |      924.5      |       500       | DR8 - DR13 |
|    3    |    down   |      925.1      |       500       | DR8 - DR13 |
|    4    |    down   |      925.7      |       500       | DR8 - DR13 |
|    5    |    down   |      926.3      |       500       | DR8 - DR13 |
|    6    |    down   |      926.9      |       500       | DR8 - DR13 |
|    7    |    down   |      927.5      |       500       | DR8 - DR13 |

## Walkthrough

This section covers a complete and thorough walkthrough for installing an AU915 sketch onto and Arduino shield and operation of an attached Dragino LoRaShield. A *Windows 10 environment* is used for the walkthrough. However, the same process can easily be achieved on Linux, however, some of the instructions will vary.

### Hardware Overview

The [Dragino LoRaShield](http://www.dragino.com/products/module/item/102-lora-shield.html) is an affordable Arduino shield for prototype development of LoRaWAN devices. The device is excellent for a prototype node. The version used in this project in the US900 version - however, this device is not specifically supported for the AU915 specification. A photo of the device is included below for reference. 

![](resources/Lora_Shield_v1.4.jpg "Dragino LoraShield v1.4")

Attaching the LoRaWAN shield to an Arduino is a very simple process. Just plug the shield into the matching pins available. Plug and play! Below is a photo of a Dragino LoRaShield attached to an Arduino.

NOTE: Make sure you have the US900 version of the Dragino LoRaShield. This information is available on the bottom of the PCB board. A box on the PCB should be ticked to illustrate the specific model, as illustrated in the photo below:

### Setup Arduino IDE

Download ant install the Arduino IDE. [These instructions for installation on Windows](https://www.arduino.cc/en/Guide/Windows) should provide enough information.

### Download Repo and Flash Arduino

This repo and the library can be added to the Arduino IDE library using a variety of methods. These are outlined below:

1. Install it using the Arduino Library manager ("Sketch" -> "Include Library" -> "Manage Libraries...")
2. Download a zipfile from github using the "Download ZIP" button and install it using the Arduino IDE ("Sketch" -> "Include Library" -> "Add .ZIP Library...")
3. Clone this git repository into your sketchbook/libraries folder. In Windows the specific directory is `C:\Users\<username>\My Documents\Arduino\libraries\<library folder>`

For more infomation on using Arduino libraries, see https://www.arduino.cc/en/Guide/Libraries.

Now we want to open the Dragino_LoRaShield_AU915.ino sketch file in the Arduino IDE. The sketch file is available in the `examples` directory in this repo. This specific sketch file is a modified version of the original sketch provided with the Dragino LoRaShield provided on [the Dragino Wiki](http://wiki.dragino.com/index.php?title=Lora_Shield) for the US900 version, available directly from [here](https://github.com/dragino/Lora/blob/master/Lora%20Shield/Examples/lora_shield_ttn-915-fix-frequency/lora_shield_ttn-915-fix-frequency.ino).

Make sure your Arduino and LoRaShield are attached to the host computer via USB. Now, upload sketch to Arduino, by selecting "Upload" which will compile and upload the sketch to the device.

Finally, open serial monitor so we can view output from the device and ensure it is functioning correctly. Set the serial monitor baud to 115200. You should see output similar to that below:

```
Starting
Packet queued for freq: 916800000
132794: EV_TXCOMPLETE (includes waiting for RX windows)
Packet queued for freq: 917000000
1515422: EV_TXCOMPLETE (includes waiting for RX windows)
Packet queued for freq: 917200000
2898055: EV_TXCOMPLETE (includes waiting for RX windows)
Packet queued for freq: 917400000
4280688: EV_TXCOMPLETE (includes waiting for RX windows)
Packet queued for freq: 917600000
5663319: EV_TXCOMPLETE (includes waiting for RX windows)
Packet queued for freq: 917800000
7045951: EV_TXCOMPLETE (includes waiting for RX windows)
Packet queued for freq: 918000000
8428658: EV_TXCOMPLETE (includes waiting for RX windows)
Packet queued for freq: 918200000
9811291: EV_TXCOMPLETE (includes waiting for RX windows)
Packet queued for freq: 916800000
11193937: EV_TXCOMPLETE (includes waiting for RX windows)
```

Specifically, in the output you should see packets queued for sending on 8 specific channels. Each channel is displayed as the frequency that the packet is sent using. The node should send a packet, then the channel should either: 1) Move to the next channel; or 2) If at the end of the channel list, jump back to the first channel. In the listing above, packets started sending on channel 8 (916.8 MHz) and cycled throught the remaining 7 channels before looping back to channel 8 again.

### LoRaWAN Library Configuration

For the sake of complete documentation, this section summarises the changes made to the original source code used in this project. To reiterate, two sources were used to create the required files for this project:

1. [Fork of the LMIC project modified for use on Arduinos](https://github.com/matthijskooijman/arduino-lmic)
2. [Arduino sketch (.ino file) for the LoRaShield provide by Dragino](https://github.com/dragino/Lora/blob/master/Lora%20Shield/Examples/lora_shield_ttn-915-fix-frequency/lora_shield_ttn-915-fix-frequency.ino)

As summary of changes to the source code are:

1. Modify the LMIC fork to use US915 frequency
2. Modify the LMIC fork US915 frquency to use AU915 channels
3. Modify the LMIC fork disableChannel function to use the correct channel step
4. Modify the Arduino sketch to disable the correct channels (to only enable band 2 in AU915)

The changes made are more thoroughly documented in the following subsections.

#### LMIC Fork

The LMIC library used is a pre-existing fork of the IMB LoRaMAC-in-C. To quote the original author: the form is 'the IBM LMIC (LoraMAC-in-C) library, slightly modified to run in the Arduino environment, allowing using the SX1272, SX1276 tranceivers and compatible modules'. This section outlines the modifications to the library have been made. Firstly, all modification have been made in the src folder, and sub-folder named lmic, as documented below:

```
Dragino_LoRaShield_Node_AU915\src\lmic
```

In `config.h` comment out line 8:

```
//#define CFG_eu868 1
```

In `config.h` uncomment line 9:

```
#define CFG_us915 1
```

This change specified that we should use the US915 specification. Howevern, even though we just changed the frequency to US915 we will not use this specification... we just hijacked this definition for ease of implementation. Therefore, we need to update the ecisting US915 MHz code for the AU915 specification. In the `lorabase.h` file, change lines 117 to 126 to:

```
enum { US915_125kHz_UPFBASE = 915200000,
       US915_125kHz_UPFSTEP =    200000,
       US915_500kHz_UPFBASE = 915900000,
       US915_500kHz_UPFSTEP =   1600000,
       US915_500kHz_DNFBASE = 923300000,
       US915_500kHz_DNFSTEP =    600000
};
enum { US915_FREQ_MIN = 915000000,
       US915_FREQ_MAX = 928000000 };
```       

The above code sets a new minimum (915000000) and maximum (928000000) frequqncy to match AU915 band 2 standard. The structure above this changes the channel creation outcome.

Now, we need to also make a small change in the `lmic.c` file. Change lines 776 to 779, from:

```
void LMIC_disableChannel (u1_t channel) {    
    if( channel < 72+MAX_XCHANNELS )        
    LMIC.channelMap[channel/4] &= ~(1<<(channel&0xF));
}
```

to:

```
void LMIC_disableChannel (u1_t channel) {
    if( channel < 72+MAX_XCHANNELS )        
    LMIC.channelMap[channel/16] &= ~(1<<(channel&0xF));
}
```

All done! That was all the changes required to the LMIC Arduino port to get the Dragino LoRaShield working in the AU915 standard.
       
#### Arduino sketch for Dragino LoRaShield

This script was derived from the original Dragino LoRaShield sketch provided on the Dragino Wiki. The sketch has a variety of modifications. However, the most important modification is the selection of channels. The sketch initialises the LoRaWAN chip on the shield and enables all channels by default. The snippet of code below demonstrates how to use a simple for loop to siable specific channels (0-7 and 16-72) to only leave the AU915 band 2 channel enabled (8-15).

```
// First, disable channels 0-7
for (int channel=0; channel<8; ++channel) {
  LMIC_disableChannel(channel);
}
// Now, disable channels 16-72 (is there 72 ??)
for (int channel=16; channel<72; ++channel) {
   LMIC_disableChannel(channel);
}
// This means only channels 8-15 are up
```

## Additional Resources

1. http://www.instructables.com/id/Use-Lora-Shield-and-RPi-to-Build-a-LoRaWAN-Gateway/
2. http://wiki.dragino.com/index.php?title=Lora_Shield
3. https://www.thethingsnetwork.org/forum/t/ttn-australian-communities/4068/10
4. https://www.thethingsnetwork.org/forum/t/lmic-node-dropped-packets/4400/9