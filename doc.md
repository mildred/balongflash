It's finally time for the public to make my very useful development. Our world is totally unjust, and for some reason the majority of useful software is written for Windows. Us Linux users, simply ignore, and you have to take care of themselves. It so happened, and our modem E3372S- standard flasher is only available under Windows. I had to correct this injustice. So, I will present a program balong_flash:

 E3372S flasher modem under Linux


 This program allows you to flash any firmware on Linux in any modem E3372S. Pre-need to put the modem in flashing mode. Command is de la au ^ godload. If you bâton firmware, the easiest way to send the command directly to the AT port via écho:

	echo -e "at^godload\r" >/dev/ttyUSB0

 If you have a modem installed HiLink-modifying firmware (-adb-TLN), the transfer mode modem firmware is possible through the BAD:

	adb connect 192.168.8.1
	adb shell 'echo -e "at^godload\r" >/dev/appvcom1'

 Or going to the modem via telnet and typing

	echo -e "at^godload\r" >/dev/appvcom1

 If you have an unmodified HiLink-flashing, the modem must first translate into debugmode (known ways to do this a lot, and I am now again will not describe them - all available in the header), and then send a command to the au ^ godload appeared AT port as described above for bâton firmware.

 After entering at^godload modem restarts and, depending on what firmware is installed on the modem at this point, formed a special broaching USB composition:
 - For firmware coller-formed composition with 3 serial ports and port firmware is older (usually / dev/ttyUSB2)
 - For firmware HiLink-formed composition with 2 serial ports and port firmware is younger (usually / dev/ttyUSB0)

 At this preliminary opreatsii end, and you can start the firmware. Attached to this post 2 files - balong_flash32 and balong_flash64. It de la and 64 bit versions proshivalschika - run the option, which is more suitable for your OS. The format of the command line for the case of complete flashing modem:

	./balong_flash -p <port firmware> <firmware file name>

 Port firmware - is one ttyUSB devices formed after the switch to flashing mode. As I said, for the dual-tracks it will port with the smallest number (usually / dev / ttyUSB0) For a three - with the largest (usually / dev / ttyUSB2).
 Name the firmware file - a file that contains the firmware that you want to write to the modem. You can directly use the .exe files of windows flasher, or specialized BIN files behind huaveevskih branded firmware archive. The program itself will understand the internal structure of the files. You can specify files to the main firmware, web interface (for HiLink) dashboards (for bâton).
 An example of a recording session the main firmware:

 $ ./balong_flash -p /dev/ttyUSB0 E3372Update_22.286.53.01.161_S_ADB_TLN_02.exe

  Parse file proshvki ...
  Found 10 sections
  Enters HDLC ...
  Protocol Version: 7200B - SKCBADZM
  Write Partition 0 - M3Boot
  Write Section 1 - M3Boot-ptable
  Write Section 2 - Fastboot
  Write Section 3 - Kernel
  Write Section 4 - VxWorks
  Write Section 5 - M3Image
  Write Section 6 - DSP
  Write Section 7 - Nvdload
  Write Section 8 - Système
  Write section 9 - APP
  Block 615 of 616

 In this case, a broaching port /dev/ttyUSB0, and writes a modified version HiLink-proshvka 22.286.53.01.161.
 Example de l'interface Web:

 $ ./balong_flash -p /dev/ttyUSB0 Update_WEBUI_16.100.05.00.03_V7R2_CPIO_Mod1.3.exe

  Parse file proshvki ...
  Found 3 sections
  Enters HDLC ...
  Protocol Version: 7200B - SKCBADZM
  Write Partition 0 - Oeminfo
  Write Section 1 - CDROMISO
  Write Section 2 - WebUI
  Block 1946 of 1947


 After recording the modem firmware should be removed from broaching mode, running the program with a key -r:
 $ ./balong_flash -p /dev/ttyUSB0 -r

  Enters HDLC ... ok
  Protocol Version: 7200B - SKCBADZM

  Restart your modem ...

 That's all. The modem will reset and begin operation with the new firmware.
 In short, once the steps of the firmware:
 - The modem to flashing mode
 - Fill the basic firmware
 - Fill dashboards or WebUI
 - Reboot the modem using the -r
 This sequence is very easily turned in the script - that is, you can write a script to alter the modem automatically without any human intervention.
 Now, what problems may be encountered. If you start the program can not initialize the protocol firmware and provides just such abuse:
  Enters HDLC ...
  Team ^ mode de données rejected an attempt to repeat ...
  Failed to get the protocol version, repeat the attempt ...
  Team ^ mode de données rejected an attempt to repeat ...
  Team ^ mode de données rejected an attempt to repeat ...
  Team ^ mode de données rejected an attempt to repeat ...

 That should stop the execution of the program (by ctrl-c) and run it again. The second time will succeed. While I have not figured out how to beat this glitch, but be sure to come up soon.
 If the modem after a distortion of power again tumble in flashing mode instead of the operating mode, then just run with the -r option programa and modem will be removed from the flashing mode.
 Sometimes dashboards / Webu not want sewn once in a single session with the main firmware. Then, after the recording of the main firmware should abstain reconnect the modem to the USB (after restarting the modem automatically enters the firmware) and then we sew the missing ingredient.
 Features of the program are not limited to full modem firmware. She cut pozaolyaet firmware file into its component sections and flash the modem only necessary set of sections. You can also modify their own sections for flashing images - is not checked header checksum, and you can flash to the modem anything. Unlike vindovyh proshivalki, there will never be any nonsense like "the port is not found" is not required pandemonium installing / reinstalling drivers, redundant interfere with the process, and other sorts mbbservice. The program simply sews the desired modem firmware.
 While the program does not support an update by checking the digital signature proshivalschika (^ signver), but I will soon add to the program and the opportunity. Then it will be flashing and the modem E3372H - as long as this program is not supported by the modem.
 The source code of the latest version of the program lies in the repository on githabe If you want to have the latest version with all the beneficial changes - download the source code from there and collect.

 Soon I will publish the methodology for disaster recovery modem and the flasher will be indispensable for the modem firmware which is in emergency mode boot.

 Github: https://github.com/forth32/balongflash
