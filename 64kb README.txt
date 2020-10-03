
Hi,

this is a short summary of my steps to deal with the unsupported URUAV TMX5 module.
I think there are no personal addons here, all info pieces could be found in this forum or on the net.
Except it took hours and some kind of logic to follow the rabit 
I write it down because I didnt found in such a collected form the whole process.
Hope it helps for people who bought this piece of ...khm... electronics  to save some time.

If my summary does anything again the will of unsupport decision I fully understand and delete this post.
Just notify me if it would be better for this great project and bad implementors should learn this lesson instead 

So the module is the URUAV TMX5 with all electronic diversions can be found in this thread (bad MCU, bad inv).
It came with fw V1.2.0.20. Initial attempt to upgrade to latest version with flash-multi resulted in verification error and
a message about 128kb limits (do not remember the exact msg). After turning off mem limit check in flasher the
image got flashed but with ugly red LED blink pattern (this was the SOS pattern which is a sw feature implemented
refusing to run on such bad devices like the TMX5 as I learned some hours later).

I downloaded the multiprotocol source from project's page and opened it using Arduino.
Very good detailed steps can be found here (follow Option 3, the Arduino way):
https://github.com/pascallanger/DIY-...iling_STM32.md

I played a bit with it's _config.h trying to reduce the compiled binary size of module
below 64kb but still saving features I wanted for my Taranis X9D+/OpenTX.
Here is what I set:
#define CHECK_FOR_BOOTLOADER
#define AETR // keep in sync with your OpenTX setting
#define REVERSE_ELEVATOR // first working rounds proofed that this is necessary, but do as you like
#define ENABLE_BIND_CH
#define BIND_CH 16
#define WAIT_FOR_BIND
#define A7105_INSTALLED
#define CYRF6936_INSTALLED
#define CC2500_INSTALLED
#define NRF24L01_INSTALLED
#define HUBSAN_A7105_INO
#define E01X_NRF24L01_INO
#define MJXQ_NRF24L01_INO
#define TELEMETRY
#define INVERT_TELEMETRY
//#define INVERT_TELEMETRY_TX
//#define MULTI_STATUS
#define MULTI_TELEMETRY
#define ENABLE_SERIAL
#define ENABLE_PPM
#define TX_ER9X
#define DISABLE_FLASH_SIZE_CHECK // the most important setting to turn of sw self protecting SOS feature mentioned above!

These settings should be made in myconfig.h-way, but it was to late when I recognized that 

I compiled the source with Arduino and exported the result file.

I connected the module to USB-to-serial dongle with some soldering. I followed the instructions here:
https://www.rcgroups.com/forums/show...r#post43814397
Maybe this wasnt neccessary at all, maybe this customized binary could be flashed via simple DFU way to.

Flashing was made using the flash-multi sw (steps in the same Compiling_STM32.md page above, but follow Option 1),
via selecting the exported binary and using the COMx port representing the USB-to-serial connection.

After these rounds the module was recognized (showing it's sw version in OpenTX's model setup) and I was
able to bind with E010 and flying it (with reversed AIL as mentioned above).

I left the soldered gnd/tx/rx/3.3v cable in TMX5 box (a lot of space inside  for later possibilities.
I used a 5pin cable to connect X9D+ with the module's connector during trials to make it easier.
I do not used the USB on the TMX5 at all in above steps just for failed trials made before all of the above.
I think the key is the custom sw binary and all other steps arent really necessary.

But this was the way which worked for me.

PS: Multiprotocol TX module is a simple but wonderful thing. I like it!
