# atulpati1. Overall Structure
RetroWatch is a simple system that is used a hardware platform called Arduino, which is intended for artists, designers, and hobbyst, and an Android app. The overall structure is below here.

Structure overview

Imagine a very small computer that you can wear on your wrist. Arduino board is a micro-processor and a storage, and there’s only one input method : a button. Bluetooth is to communicate with other devices, and the battery would be necessary for power. I’ll install an Android app for collecting or editing various RSS and system information and for notification on the Android device. This app will also process the data because the Arduino board has very limited resources.

 

2. Preparing for RetroWatch
You need to prepare modules as small as possible to wear the watch on your wrist. There may be tons of variations, but I chose parts that are commonly used and inexpensive. You can replace modules to the others if you want, and you can even use source codes as they are in case you use Arduino-compatible boards. But do not forget you have to change pin numbers that are connected with the parts.

2-1. Hardware for smart watch
before assembly

2-1-1. Arduino micro-controller
There are tons of Arduino boards, but I chose the smallest one, Pro mini. Arduino Pro mini is a light version of UNO R3. It doesn’t have a USB interface chip in order to reduce the price and the size. There are two version depending on the operating voltage(3.3v/5v). And I used a 3.3v version, because the bluetooth and display that are connected with the board support 3.3v and the board goes well with the 3.7v LiPo battery. It runs at 8MHz and a 5v version at 16MHz, but 8MHz is enough. Overall, all you need to prepare are Arduino Pro Mini 3.3v and USB to UART module.

The main chip of the board, ATmega328, has only 2KBytes RAM, but ATmega128 has just 1KBytes, which is very limited to run my system. Most boards have ATmega328, but you have to make sure.
2-1-2. Bluetooth
Most common bluetooth modules that you can get are HC-06 main module and the one with interface base board. The latter one has a reset button, the status LED, and it supports both operation voltage(3,3v/5v), so this one is more convenient but the size is rather big, the LED, which is not quite necessary drains the battery and a little more expensive. So I used a HC-06 without the interface board. size: 28mm x 15 mm x 2.35mm (35.7mm x 15.2mm with base board)

2-1-3. Display
The core part would be a display. To make a SMART WATCH, it would be necessary to find a small, low-power display. I’d given lots of thoughts, then I chose -.96’’ OLED Display. size: 26.7mm x 19.26mm x ??

There are various sizes of the displays(0.96’’, 1.3’’, etc), it works on low-power, English font and the image out available, and it supports I2C, SPI, which makes easy to connect with Arduino. The pros and cons is the graphic library. The library for drawing shapes, fonts, and images is supported, but it requires many RAM(I guess the graphic buffer is the problem). So I need to be very stingy in terms of memory usage when I make a program.

Be careful at selecting a display!! I used an 128×64 OLED which is using I2C and SSD1306 driver chip. If you are using different one, you may need to use different graphic libraries and modify arduino source code.

Updated (2015.01.12) : RetroWatch supports u8glib. Now you can use various kind of OLED(or else). Check supported devices at this link.

2-1-4. Battery
I use LiPo(Lithum-Polymer) battery in this project. 1-cell LiPo battery flows out current in 3.7v, which works perfectly with Arduino Pro mini, and there are many kind of batteries in terms of the size and the capacity. You might need to try with batteries with different capacity. I used 501430 – 170mAh, 302030 – 140mAh, 552036 – 350mAh when I made RetroWatch prototype(The first two digits refer to the depth, next two are the width, the last two are the height). The uptime for 501430 – 170mAh and 302030 – 140mAh with periodical sync and no user interaction is :501430 – 170mAh – 7h 30min, 302030 – 140mAh – less than 7h. The most ideal battery depends on what size of watch you want to make. The choice is yours. Under 100mAh one is small, but it doesn’t guarantee stable power, and if it’s too low, you can’t even boot the system. I recommend the battery with protection circuit(overcharging, over-discharging safe), and it’s better if it has a removable socket. It might helpful for you if you get a female socket and USB or 220v DC adaptor recharger.

2-1-5. Etc.
You need wires, soldering iron, a switch and 10K ohm resistance(for a button), and a batter jack. It would be helpful for you to prepare the assembly manual.

In my case, I stacked modules like below picture.

assembly

 

2-2. Preparing for Android
Android v.4.3 supports the service that is used to get notification information from an app. So RetroWatch app is based on Android v.4.3 to enable notification service. For users who don’t use Android v.4.3 yet, the app without this function is released also. You can download the sources at GitHub, or download the app from Google Play Store.(Search with “RetroWatch” or “RetroWatch LE“)

 

3. Assembling the watch
Folks who are accustomed to handling Arduino or physical computing would proceed this procedure, but I recommend the others not to assemble Arduino Pro mini board first, but try to make the watch with the board that is easy to connect and use like UNO board. The assembly structure for RetroWatch is below here.

Compare your stuff with mine in the picture below. Beware not to fuse, and make sure each layer keep isolated with each other by using tape.

3-1. Connecting Arduino – bluetooth
You may refer to the website for the common way of Arduino – bluetooth connection and test.(but in Korean) You can use the device name and password as is or after resetting them. In the link, you can see the bluetooth module that is connected with the interface board, but the connecting is very similar.

BT -> Arduino :
VCC -> 3.3V,
GND -> GND,
TX -> D2,
RX -> D3,

bluetooth

There’s a report that says the bluetooth modules can malfunction if they contact with other conducting materials or antennas.

3-2. Connecting Arduino – OLED
The OLED that is used on RetroWatch communicate with the Arduino board by I2C. The common connection of I2C interface is below, and you just follow the instruction.

OLED -> Arduino Pro mini :
GND -> GND,
VCC -> VCC,
SDA -> A4(the analog 4th pin),
SCL -> A5(the analog 5th pin),

If your display has SPI interface, refer to the link. In case of 7pin SPI OLED, connect like below.

D1 : MOSI – Arduino D11 (MOSI)
D2 : MISO – Arduino D12 (MISO) : this pin is optional.
D0 : CLK – Arduino D13 (SCK)
DC : DC (Data Command) – Arduino D8 (or else)
CS : CS (Chip select) – Arduino D10 (SS)
RES : RESET – Arduino D9 (or else)
3-3. Connecting Arduino – button
You may connect with a button which is small enough for the smart watch and a 10k-ohm resistance as below. Connect a digital pin and modify the pin number which is defined as “buttonPin” on the source code. I used a digital 5 pin. In order to refer to button control, you can find the examples about switch control by googling.

button

3-4. Connecting Arduino – battery
You can simply connect battery by connecting (+) -> RAW, GND -> GND. I chose a LiPo recharging battery, so I left 2 wires and connected them to a female socket. You need to put (+) line to RAW pin in case you use any sort of external power supply, such as an external battery, on Arduino Pro mini board. Otherwise, it can damage the board.

3-5. Connecting Arduino – UART module
You should connect USB to UART to the board in order to upload the source code by USB serial connection. You can read the details here. It depends on the modules how they connect to the board, but normally, you should cross-connect RX-TX.

USB to UART module -> Arduino (Pro mini): ,
3.3V -> VCC,
TXD -> RXD,
RXD -> TXD,
GND -> GND,

3-6. Checking connection
The result is as below.

RetroWatch_circuit_all

 

 

 

Now, it’s time for checking the connection. If you connect FTDI module(USB to UART, upper-left in the picture) to PC and the light is on, the boot process is successfully done. The following pictures are the assembly procedure and RetroWatch that is completely assembled.

assembly_process
