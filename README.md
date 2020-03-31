# Firmware-Analysis
IT Forensics Final Project Blog Post
hello
<html><head><meta content="text/html; charset=UTF-8" http-equiv="content-type"><style type="text/css">ol{margin:0;padding:0}table td,table th{padding:0}.c6{padding-top:18pt;padding-bottom:6pt;line-height:1.15;page-break-after:avoid;orphans:2;widows:2;text-align:left}.c7{color:#000000;font-weight:400;text-decoration:none;vertical-align:baseline;font-size:16pt;font-family:"Arial";font-style:normal}.c1{padding-top:0pt;padding-bottom:0pt;line-height:1.15;orphans:2;widows:2;text-align:left;height:11pt}.c3{padding-top:14pt;padding-bottom:4pt;line-height:1.15;page-break-after:avoid;orphans:2;widows:2;text-align:left}.c5{color:#666666;font-weight:400;text-decoration:none;vertical-align:baseline;font-size:12pt;font-family:"Arial";font-style:normal}.c0{color:#000000;font-weight:400;text-decoration:none;vertical-align:baseline;font-size:11pt;font-family:"Arial";font-style:normal}.c2{padding-top:0pt;padding-bottom:0pt;line-height:1.15;orphans:2;widows:2;text-align:left}.c8{background-color:#ffffff;max-width:468pt;padding:72pt 72pt 72pt 72pt}.c4{margin-left:36pt}.title{padding-top:0pt;color:#000000;font-size:26pt;padding-bottom:3pt;font-family:"Arial";line-height:1.15;page-break-after:avoid;orphans:2;widows:2;text-align:left}.subtitle{padding-top:0pt;color:#666666;font-size:15pt;padding-bottom:16pt;font-family:"Arial";line-height:1.15;page-break-after:avoid;orphans:2;widows:2;text-align:left}li{color:#000000;font-size:11pt;font-family:"Arial"}p{margin:0;color:#000000;font-size:11pt;font-family:"Arial"}h1{padding-top:20pt;color:#000000;font-size:20pt;padding-bottom:6pt;font-family:"Arial";line-height:1.15;page-break-after:avoid;orphans:2;widows:2;text-align:left}h2{padding-top:18pt;color:#000000;font-size:16pt;padding-bottom:6pt;font-family:"Arial";line-height:1.15;page-break-after:avoid;orphans:2;widows:2;text-align:left}h3{padding-top:16pt;color:#434343;font-size:14pt;padding-bottom:4pt;font-family:"Arial";line-height:1.15;page-break-after:avoid;orphans:2;widows:2;text-align:left}h4{padding-top:14pt;color:#666666;font-size:12pt;padding-bottom:4pt;font-family:"Arial";line-height:1.15;page-break-after:avoid;orphans:2;widows:2;text-align:left}h5{padding-top:12pt;color:#666666;font-size:11pt;padding-bottom:4pt;font-family:"Arial";line-height:1.15;page-break-after:avoid;orphans:2;widows:2;text-align:left}h6{padding-top:12pt;color:#666666;font-size:11pt;padding-bottom:4pt;font-family:"Arial";line-height:1.15;page-break-after:avoid;font-style:italic;orphans:2;widows:2;text-align:left}</style></head><body class="c8"><h2 class="c6" id="h.1g9vm8gtk2z8"><span class="c7">Demo</span></h2><p class="c2"><span class="c0">In this section of the report, we will go through the full firmware analysis and extraction process. We will utilize the discussed tools such as binwalk, DD, firmwalker, strings and a decompression tool. We will discuss two approaches to extract the filesystem, the first using DD and the second using binwalk. </span></p><p class="c1"><span class="c0"></span></p><p class="c1"><span class="c0"></span></p><h4 class="c3" id="h.5pk2mqd1jiqb"><span class="c5">Choose Device</span></h4><p class="c2"><span class="c0">The first step of this process is to choose an IoT device to analyze. We have chosen the D-Link DCS-5020L network camera [14]. This $120 MSRP camera has many features such as a wide viewing range with pan/tilt, mobile app functionality and can even function as a Wi-Fi extender [15]. </span></p><p class="c2"><span style="overflow: hidden; display: inline-block; margin: 0.00px 0.00px; border: 0.00px solid #000000; transform: rotate(0.00rad) translateZ(0px); -webkit-transform: rotate(0.00rad) translateZ(0px); width: 412.43px; height: 313.50px;"><img alt="" src="images/image17.png" style="width: 412.43px; height: 313.50px; margin-left: 0.00px; margin-top: 0.00px; transform: rotate(0.00rad) translateZ(0px); -webkit-transform: rotate(0.00rad) translateZ(0px);" title=""></span></p><p class="c2"><span class="c0">Figure X - D-Link DCS-5020L Network Camera [14]</span></p><p class="c1"><span class="c0"></span></p><h4 class="c3" id="h.99caa9wrc6ad"><span class="c5">Acquire Firmware File</span></h4><p class="c2"><span class="c0">Next, we need to acquire the firmware file for this IoT camera. As this device is updated manually, we need to go to the D-Link support site and download the firmware file, as per Figure X. </span></p><p class="c2"><span style="overflow: hidden; display: inline-block; margin: 0.00px 0.00px; border: 0.00px solid #000000; transform: rotate(0.00rad) translateZ(0px); -webkit-transform: rotate(0.00rad) translateZ(0px); width: 624.00px; height: 324.00px;"><img alt="" src="images/image3.png" style="width: 624.00px; height: 324.00px; margin-left: 0.00px; margin-top: 0.00px; transform: rotate(0.00rad) translateZ(0px); -webkit-transform: rotate(0.00rad) translateZ(0px);" title=""></span></p><p class="c2"><span class="c0">Figure X - Download firmware file for D-Link DCS-5020L [16]</span></p><p class="c1"><span class="c0"></span></p><h4 class="c3" id="h.u1l2tka8jbt1"><span class="c5">Determine File Type</span></h4><p class="c2"><span class="c0">Next, we need to learn a little bit more about the firmware file we have downloaded. We want to make sure it isn&rsquo;t compressed, encrypted or corrupted. We can do this by running the file command on the firmware file and the output of the file command is shown at Figure X. It appears that the firmware file is not corrupt or encrypted, and it is for the 5020L IoT Camera. </span></p><p class="c1"><span class="c0"></span></p><p class="c2"><span style="overflow: hidden; display: inline-block; margin: 0.00px 0.00px; border: 0.00px solid #000000; transform: rotate(0.00rad) translateZ(0px); -webkit-transform: rotate(0.00rad) translateZ(0px); width: 624.00px; height: 53.33px;"><img alt="" src="images/image9.png" style="width: 624.00px; height: 53.33px; margin-left: 0.00px; margin-top: 0.00px; transform: rotate(0.00rad) translateZ(0px); -webkit-transform: rotate(0.00rad) translateZ(0px);" title=""></span></p><p class="c2"><span class="c0">Figure X - Output of the file command</span></p><p class="c1"><span class="c0"></span></p><h4 class="c3" id="h.j8zck73k1wrp"><span class="c5">Analyze the image using Binwalk</span></h4><p class="c2"><span>The next step is to analyze the image using a binwalk. This will tell us some important details about the content of the image. As per Figure X, There appears to be a </span><span>UBOOT</span><span class="c0">&nbsp;bootloader at the 99360 offset. The OS is Linux and it is running a MIPS instruction set. At offset 327744 there is some LZMA compressed data, which is the file system. The uImage header above it says it is a OS Kernel Image compressed with LZMA. </span></p><p class="c1"><span class="c0"></span></p><p class="c1"><span class="c0"></span></p><p class="c1"><span class="c0"></span></p><p class="c1"><span class="c0"></span></p><p class="c1"><span class="c0"></span></p><p class="c1"><span class="c0"></span></p><p class="c1"><span class="c0"></span></p><p class="c1"><span class="c0"></span></p><p class="c2"><span class="c0">Figure X - Running the Binwalk command on the binary file. </span><span style="overflow: hidden; display: inline-block; margin: 0.00px 0.00px; border: 0.00px solid #000000; transform: rotate(0.00rad) translateZ(0px); -webkit-transform: rotate(0.00rad) translateZ(0px); width: 775.98px; height: 230.50px;"><img alt="" src="images/image16.png" style="width: 775.98px; height: 230.50px; margin-left: 0.00px; margin-top: 0.00px; transform: rotate(0.00rad) translateZ(0px); -webkit-transform: rotate(0.00rad) translateZ(0px);" title=""></span></p><p class="c1"><span class="c0"></span></p><p class="c1"><span class="c0"></span></p><p class="c1"><span class="c0"></span></p><h4 class="c3" id="h.jgozly1gwfob"><span class="c5">Entropy Analysis Before Extraction</span></h4><p class="c2"><span>Before we extract the file, we can do entropy analysis to determine if the entire file system is compressed. We can use Binwalk with the &ldquo;-e&rdquo; flag to do this. Running the command in Figure X displays an entropy plot as shown in Figure X. As per the entropy plot, the entire firmware file appears to have a high entropy, which makes sense, as the file system is compressed. </span><span style="overflow: hidden; display: inline-block; margin: 0.00px 0.00px; border: 0.00px solid #000000; transform: rotate(0.00rad) translateZ(0px); -webkit-transform: rotate(0.00rad) translateZ(0px); width: 624.00px; height: 113.33px;"><img alt="" src="images/image6.png" style="width: 624.00px; height: 113.33px; margin-left: 0.00px; margin-top: 0.00px; transform: rotate(0.00rad) translateZ(0px); -webkit-transform: rotate(0.00rad) translateZ(0px);" title=""></span></p><p class="c2"><span class="c0">Figure X - Determining Entropy of firmware file using binwalk</span></p><p class="c1"><span class="c0"></span></p><p class="c2 c4"><span style="overflow: hidden; display: inline-block; margin: 0.00px 0.00px; border: 0.00px solid #000000; transform: rotate(0.00rad) translateZ(0px); -webkit-transform: rotate(0.00rad) translateZ(0px); width: 438.30px; height: 350.50px;"><img alt="" src="images/image14.png" style="width: 438.30px; height: 350.50px; margin-left: 0.00px; margin-top: 0.00px; transform: rotate(0.00rad) translateZ(0px); -webkit-transform: rotate(0.00rad) translateZ(0px);" title=""></span></p><p class="c2"><span class="c0">Figure X - Examining the entropy plot of the file</span></p><p class="c1"><span class="c0"></span></p><p class="c1"><span class="c0"></span></p><h4 class="c3" id="h.pkug83azi091"><span class="c5">Extracting the compressed file system using DD</span></h4><p class="c2"><span class="c0">The next step is to extract the file system from the firmware file. To do this, we need to provide an input file, output file, bytes to skip, byte size and number of bytes. The complete command can be seen in figure Z. The skip and count fields are determined from the binwalk output, and the bytesize (bs) field can remain as 1. </span></p><p class="c2 c4"><span style="overflow: hidden; display: inline-block; margin: 0.00px 0.00px; border: 0.00px solid #000000; transform: rotate(0.00rad) translateZ(0px); -webkit-transform: rotate(0.00rad) translateZ(0px); width: 624.00px; height: 44.00px;"><img alt="" src="images/image11.png" style="width: 624.00px; height: 44.00px; margin-left: 0.00px; margin-top: 0.00px; transform: rotate(0.00rad) translateZ(0px); -webkit-transform: rotate(0.00rad) translateZ(0px);" title=""></span></p><p class="c2"><span class="c0">Figure Z - Complete DD command to extract file system</span></p><p class="c1"><span class="c0"></span></p><p class="c2"><span class="c0">The skip field can be determined using the &ldquo;decimal&rdquo; or offset value in the original binwalk output, as highlighted in Figure X. This value is 327744. </span></p><p class="c1"><span class="c0"></span></p><p class="c2 c4"><span style="overflow: hidden; display: inline-block; margin: 0.00px 0.00px; border: 0.00px solid #000000; transform: rotate(0.00rad) translateZ(0px); -webkit-transform: rotate(0.00rad) translateZ(0px); width: 624.00px; height: 313.33px;"><img alt="" src="images/image13.png" style="width: 624.00px; height: 313.33px; margin-left: 0.00px; margin-top: 0.00px; transform: rotate(0.00rad) translateZ(0px); -webkit-transform: rotate(0.00rad) translateZ(0px);" title=""></span></p><p class="c2"><span class="c0">Figure X - Highlighted Skip value for compressed file system extraction</span></p><p class="c1"><span class="c0"></span></p><p class="c2"><span class="c0">The count is the image size, which can also be determined by the original binwalk output. This value is highlighted in Figure X and is 7301445. </span></p><p class="c2"><span class="c0">Figure X - Highlighted image size filed used for count parameter in DD command</span><span style="overflow: hidden; display: inline-block; margin: 0.00px 0.00px; border: 0.00px solid #000000; transform: rotate(0.00rad) translateZ(0px); -webkit-transform: rotate(0.00rad) translateZ(0px); width: 794.41px; height: 172.50px;"><img alt="" src="images/image4.png" style="width: 794.41px; height: 172.50px; margin-left: 0.00px; margin-top: 0.00px; transform: rotate(0.00rad) translateZ(0px); -webkit-transform: rotate(0.00rad) translateZ(0px);" title=""></span></p><p class="c1"><span class="c0"></span></p><p class="c1"><span class="c0"></span></p><h4 class="c3" id="h.yzmj9j4w1e6"><span class="c5">Decompress the extracted file</span></h4><p class="c2"><span class="c0">Now that we have extracted the file system, the next step is to decompress it. As per the previous binwalk output, we have determined that the LZMA file compression algorithm was used. The file can be extracted using the command as per Figure X. </span></p><p class="c1"><span class="c0"></span></p><p class="c2 c4"><span style="overflow: hidden; display: inline-block; margin: 0.00px 0.00px; border: 0.00px solid #000000; transform: rotate(0.00rad) translateZ(0px); -webkit-transform: rotate(0.00rad) translateZ(0px); width: 624.00px; height: 33.33px;"><img alt="" src="images/image12.png" style="width: 624.00px; height: 33.33px; margin-left: 0.00px; margin-top: 0.00px; transform: rotate(0.00rad) translateZ(0px); -webkit-transform: rotate(0.00rad) translateZ(0px);" title=""></span></p><p class="c2 c4"><span class="c0">Figure X - Decompressing extracted file system using the unlzma utility</span></p><h4 class="c3" id="h.v6doi5i926hj"><span class="c5">Analyze File System Using the Strings Utility</span></h4><p class="c2"><span class="c0">Now that we have extracted and decompressed the file system, we can use the strings utility to search for interesting things. For example, we can determine the type of file system by searching for known file system keywords such as &ldquo;file system&rdquo;, &ldquo;CIFS&rdquo; or &ldquo;squashfs&rdquo;. Figure X confirms that the DCS-5020L IoT camera uses the SquashFS file system. We can further confirm this by searching specifically for squashfs, as per figure X. </span></p><p class="c1"><span class="c0"></span></p><p class="c2 c4"><span style="overflow: hidden; display: inline-block; margin: 0.00px 0.00px; border: 0.00px solid #000000; transform: rotate(0.00rad) translateZ(0px); -webkit-transform: rotate(0.00rad) translateZ(0px); width: 624.00px; height: 221.33px;"><img alt="" src="images/image8.png" style="width: 624.00px; height: 221.33px; margin-left: 0.00px; margin-top: 0.00px; transform: rotate(0.00rad) translateZ(0px); -webkit-transform: rotate(0.00rad) translateZ(0px);" title=""></span></p><p class="c2"><span class="c0">Figure X - Determining Filesystem type. </span></p><p class="c1"><span class="c0"></span></p><p class="c1"><span class="c0"></span></p><p class="c2"><span style="overflow: hidden; display: inline-block; margin: 0.00px 0.00px; border: 0.00px solid #000000; transform: rotate(0.00rad) translateZ(0px); -webkit-transform: rotate(0.00rad) translateZ(0px); width: 624.00px; height: 200.00px;"><img alt="" src="images/image1.png" style="width: 624.00px; height: 200.00px; margin-left: 0.00px; margin-top: 0.00px; transform: rotate(0.00rad) translateZ(0px); -webkit-transform: rotate(0.00rad) translateZ(0px);" title=""></span></p><p class="c2"><span class="c0">Figure X - Confirming file system using the Strings utility. </span></p><p class="c1"><span class="c0"></span></p><p class="c1"><span class="c0"></span></p><h4 class="c3" id="h.qtvmvg6jg6wz"><span class="c5">Extracting File System Using Binwalk</span></h4><p class="c2"><span class="c0">Using the &ldquo;eM&rdquo; flags in Binwalk, we are able to extract the root filesystem of the camera. The &ldquo;-e&rdquo; flag will extract all files identified during the initial file signature scan. The &ldquo;-M&rdquo; flag will recursively scan extracted files. This is shown in figure X. After this command is executed, you will need to traverse multiple directories until the &ldquo;cpio-root&rdquo; directory is found, which will contain the root directory of the file system, as shown in Figure Y. </span></p><p class="c1"><span class="c0"></span></p><p class="c2 c4"><span style="overflow: hidden; display: inline-block; margin: 0.00px 0.00px; border: 0.00px solid #000000; transform: rotate(0.00rad) translateZ(0px); -webkit-transform: rotate(0.00rad) translateZ(0px); width: 624.00px; height: 94.67px;"><img alt="" src="images/image15.png" style="width: 624.00px; height: 94.67px; margin-left: 0.00px; margin-top: 0.00px; transform: rotate(0.00rad) translateZ(0px); -webkit-transform: rotate(0.00rad) translateZ(0px);" title=""></span></p><p class="c2 c4"><span class="c0">Figure X - Extracting all files using Binwalk</span></p><p class="c1 c4"><span class="c0"></span></p><p class="c1 c4"><span class="c0"></span></p><p class="c2"><span style="overflow: hidden; display: inline-block; margin: 0.00px 0.00px; border: 0.00px solid #000000; transform: rotate(0.00rad) translateZ(0px); -webkit-transform: rotate(0.00rad) translateZ(0px); width: 624.00px; height: 176.00px;"><img alt="" src="images/image5.png" style="width: 624.00px; height: 176.00px; margin-left: 0.00px; margin-top: 0.00px; transform: rotate(0.00rad) translateZ(0px); -webkit-transform: rotate(0.00rad) translateZ(0px);" title=""></span></p><p class="c2"><span class="c0">Figure Y - Root Directory of firmware file</span></p><p class="c1"><span class="c0"></span></p><p class="c1"><span class="c0"></span></p><p class="c1"><span class="c0"></span></p><h4 class="c3" id="h.pylla5guk7dl"><span class="c5">Searching the filesystem using Firmwalker</span></h4><p class="c2"><span class="c0">Now that we have the root directory extracted, we can use Firmwalker to analyze the firmware file for interesting things such as passwords, private keys, emails, IP&rsquo;s, etc. Figure X displays the process of using firmwalker. To use this tool, you must download the script from the Firmwalker github page, and point it to the root directory of the firmware file (the previously extracted cpio-root directory). </span></p><p class="c1"><span class="c0"></span></p><p class="c2"><span style="overflow: hidden; display: inline-block; margin: 0.00px 0.00px; border: 0.00px solid #000000; transform: rotate(0.00rad) translateZ(0px); -webkit-transform: rotate(0.00rad) translateZ(0px); width: 624.00px; height: 37.33px;"><img alt="" src="images/image10.png" style="width: 624.00px; height: 37.33px; margin-left: 0.00px; margin-top: 0.00px; transform: rotate(0.00rad) translateZ(0px); -webkit-transform: rotate(0.00rad) translateZ(0px);" title=""></span></p><p class="c2"><span class="c0">Figure X - Running firmwalker against root directory of firmware</span></p><p class="c1"><span class="c0"></span></p><p class="c2"><span class="c0">Firmwalker produces lots of output, however some interesting findings are present in Figure Y. It appears the camera has the telnet binaries installed and it has found some private keys. </span></p><p class="c2"><span style="overflow: hidden; display: inline-block; margin: 0.00px 0.00px; border: 0.00px solid #000000; transform: rotate(0.00rad) translateZ(0px); -webkit-transform: rotate(0.00rad) translateZ(0px); width: 624.00px; height: 377.33px;"><img alt="" src="images/image7.png" style="width: 624.00px; height: 377.33px; margin-left: 0.00px; margin-top: 0.00px; transform: rotate(0.00rad) translateZ(0px); -webkit-transform: rotate(0.00rad) translateZ(0px);" title=""></span></p><p class="c2"><span class="c0">Figure Y - Telnet configuration and private keys. </span></p><p class="c1"><span class="c0"></span></p><p class="c2"><span>If we take it one step further, we can find the private key of the server in /etc_ro/</span><span>serverkey.pem</span><span class="c0">, as shown in Figure Z. </span></p><p class="c1"><span class="c0"></span></p><p class="c2"><span style="overflow: hidden; display: inline-block; margin: 0.00px 0.00px; border: 0.00px solid #000000; transform: rotate(0.00rad) translateZ(0px); -webkit-transform: rotate(0.00rad) translateZ(0px); width: 624.00px; height: 309.33px;"><img alt="" src="images/image2.png" style="width: 624.00px; height: 309.33px; margin-left: 0.00px; margin-top: 0.00px; transform: rotate(0.00rad) translateZ(0px); -webkit-transform: rotate(0.00rad) translateZ(0px);" title=""></span></p><p class="c2"><span class="c0">Figure Z - Private key</span></p><p class="c1"><span class="c0"></span></p><p class="c1"><span class="c0"></span></p></body></html>

