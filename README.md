# IT Forensics Final Project Blog Post



### Demo
#### In this section of the report, we will go through the full firmware analysis and extraction process. We will utilize the discussed tools such as binwalk, DD, firmwalker, strings and a decompression tool. We will discuss two approaches to extract the filesystem, the first using DD and the second using binwalk. 
Choose Device
The first step of this process is to choose an IoT device to analyze. We have chosen the D-Link DCS-5020L network camera [14]. This $120 MSRP camera has many features such as a wide viewing range with pan/tilt, mobile app functionality and can even function as a Wi-Fi extender [15]. 

Figure X - D-Link DCS-5020L Network Camera [14]

Acquire Firmware File
Next, we need to acquire the firmware file for this IoT camera. As this device is updated manually, we need to go to the D-Link support site and download the firmware file, as per Figure X. 

Figure X - Download firmware file for D-Link DCS-5020L [16]

Determine File Type
Next, we need to learn a little bit more about the firmware file we have downloaded. We want to make sure it isn’t compressed, encrypted or corrupted. We can do this by running the file command on the firmware file and the output of the file command is shown at Figure X. It appears that the firmware file is not corrupt or encrypted, and it is for the 5020L IoT Camera. 


Figure X - Output of the file command

Analyze the image using Binwalk
The next step is to analyze the image using a binwalk. This will tell us some important details about the content of the image. As per Figure X, There appears to be a UBOOT bootloader at the 99360 offset. The OS is Linux and it is running a MIPS instruction set. At offset 327744 there is some LZMA compressed data, which is the file system. The uImage header above it says it is a OS Kernel Image compressed with LZMA. 








Figure X - Running the Binwalk command on the binary file. 



Entropy Analysis Before Extraction
Before we extract the file, we can do entropy analysis to determine if the entire file system is compressed. We can use Binwalk with the “-e” flag to do this. Running the command in Figure X displays an entropy plot as shown in Figure X. As per the entropy plot, the entire firmware file appears to have a high entropy, which makes sense, as the file system is compressed. 
Figure X - Determining Entropy of firmware file using binwalk


Figure X - Examining the entropy plot of the file


Extracting the compressed file system using DD
The next step is to extract the file system from the firmware file. To do this, we need to provide an input file, output file, bytes to skip, byte size and number of bytes. The complete command can be seen in figure Z. The skip and count fields are determined from the binwalk output, and the bytesize (bs) field can remain as 1. 

Figure Z - Complete DD command to extract file system

The skip field can be determined using the “decimal” or offset value in the original binwalk output, as highlighted in Figure X. This value is 327744. 


Figure X - Highlighted Skip value for compressed file system extraction

The count is the image size, which can also be determined by the original binwalk output. This value is highlighted in Figure X and is 7301445. 
Figure X - Highlighted image size filed used for count parameter in DD command


Decompress the extracted file
Now that we have extracted the file system, the next step is to decompress it. As per the previous binwalk output, we have determined that the LZMA file compression algorithm was used. The file can be extracted using the command as per Figure X. 


Figure X - Decompressing extracted file system using the unlzma utility
Analyze File System Using the Strings Utility
Now that we have extracted and decompressed the file system, we can use the strings utility to search for interesting things. For example, we can determine the type of file system by searching for known file system keywords such as “file system”, “CIFS” or “squashfs”. Figure X confirms that the DCS-5020L IoT camera uses the SquashFS file system. We can further confirm this by searching specifically for squashfs, as per figure X. 


Figure X - Determining Filesystem type. 



Figure X - Confirming file system using the Strings utility. 


Extracting File System Using Binwalk
Using the “eM” flags in Binwalk, we are able to extract the root filesystem of the camera. The “-e” flag will extract all files identified during the initial file signature scan. The “-M” flag will recursively scan extracted files. This is shown in figure X. After this command is executed, you will need to traverse multiple directories until the “cpio-root” directory is found, which will contain the root directory of the file system, as shown in Figure Y. 


Figure X - Extracting all files using Binwalk



Figure Y - Root Directory of firmware file



Searching the filesystem using Firmwalker
Now that we have the root directory extracted, we can use Firmwalker to analyze the firmware file for interesting things such as passwords, private keys, emails, IP’s, etc. Figure X displays the process of using firmwalker. To use this tool, you must download the script from the Firmwalker github page, and point it to the root directory of the firmware file (the previously extracted cpio-root directory). 


Figure X - Running firmwalker against root directory of firmware

Firmwalker produces lots of output, however some interesting findings are present in Figure Y. It appears the camera has the telnet binaries installed and it has found some private keys. 

Figure Y - Telnet configuration and private keys. 

If we take it one step further, we can find the private key of the server in /etc_ro/serverkey.pem, as shown in Figure Z. 


Figure Z - Private key
