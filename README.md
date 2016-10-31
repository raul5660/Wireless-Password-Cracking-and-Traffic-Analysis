# Wireless Password Cracking and Traffic Analysis

## Introduction

Before I begin I would like to make sure the readers understand these techniques are not to be used outside of research or unless explicitly given the authority to do so. These items are provided for educational purposes and were prepared in a simulated environment. This means no animal, person, hardware, network, etc. were injured or infiltrated.

Everyone utilizes some type of wireless technology. Our cellphones, laptops, tablets, PC's, and even our cars are connected to the internet in one-way shape (literally shape) or form. Cellphones and vehicles utilize 4G/LTE technology, but in this tutorial we will focus on Wi-Fi technology. In most cases when you connect to a wireless access point or router you have a password to enter in order to use it. This is a good start because when you connect to the internet the radio signals sent and received are encrypted with the password. You can think of it as a lock box you and the router have a key to. Any message in this lock box can only be read by you and your router. The problem with this is the fact most people use the default password that comes with your router. This increases the risk of your data being captured because most default passwords for any given device can be found online. You may be asking how anyone could possibly get a key to your lock box even if you created a custom key (password). This is true, but what if you lost your key? You'd call a lock smith to attempt to open it. In this case we are the lock smith you did not call. For security reasons the method in which to retrieve a packet capture from wireless signals will be left out.

Let’s get started. The tools referenced in this tutorial can be found at the following locations:
* https://wireshark.com/
    * Wireshark is used to analyze packets.
* http://aircrack-ng.org/
    * Aircrack-ng is a tool used to crack wireless passwords

Next you will need to download the file created specifically for this tutorial:
* http://extra.rwdcr.com/Cersei-01.cap

To make things interesting we will have the following challenges or goals to achieve.
* Identify the wireless key (password) for Cersei-01.cap file.
* Identify a host's IP address from Domain Naming Service (DNS) traffic.

In order to determine the IP address from a DNS packet we have to understand how DNS works and what it is. DNS is a protocol used to convert a given name into an IP address a computer can understand. What does this all mean? For example, let’s say you open up a web browser (Internet Explorer, Firefox, Google Chrome, etc.) and type in google.com in the address bar. Your computer isn't as smart as you think. It doesn’t understand what google.com is. Instead, to bridge the gap the DNS protocol is used. Your web browser will tell your Operating System (OS) to determine what google.com is. Your computer then utilizes a DNS server which contains a list of domain names  to convert to an IP address. Once you ask the DNS server for the IP address it'll return the IP address of the domain name it was given.

## Tool Installation
* For the rest of the Linux tutorial we will heavily utilize the terminal. To open the terminal, follow the steps below:
	* Linux
		* Press the Windows buttons on your keyboard (between control and alt)
		![alt text][windowskey]
		* This will bring you a search window where you will type 'terminal'
		* Hit enter on your keyboard and you will get a terminal window.
	* Mac OS X
		* Press the Command and Space Bar buttons at the same time
		* Next you will be presented a search bar where you will type terminal
		* Click Enter and a terminal window should open up
* Aircrack-ng
	* To install aircrack-ng on a Linux or Apple computer is very simple. In the terminal enter the following commands:
		* Ubuntu
			* `sudo apt-get install aircrack-ng`
		* Mac OS X
			* `brew install aircrack-ng`
				* Please note you will have to install the brew package manager for Mac OS. A link is provided to the installation process for your convenience   
* Wireshark 
	* In the terminal enter the following commands for the appropriate operating system.
		* Ubuntu
			* `sudo apt-get install Wireshark`
		* Mac OS X
			* `brew install Wireshark`

## Wireless Password Cracking with aircrack-ng
* Within the terminal type `aircrack-ng`. You will notice output providing some guidance if help is needed.
* Next in the terminal we will make a directory to download the packet capture.
	* Type `mkdir tutorial` and click enter
	* Next we will move into the tutorial directory with `cd tutorial`
* Download the packet capture from the web server
	* Type `wget http://extra.rwdcr.com/Cersei-01.cap` and click enter. You will notice a bar showing the status of the download.
* Here we will attempt to crack the password on the packet capture. Enter the command `aircrack-ng Cersei-01.cap`. You will begin seeing something weird on the prompt. Have no fear. It is attempting several different passwords against the packet capture. (This should go by pretty quickly.) After finally cracking the password we can begin figuring out what data was captured.

## Identifying Traffic with Wireshark
* Please note, from here on I will be using the Mac OS X environment. Any differences between the Mac OS X and Linux Wireshark will be explicitly mentioned.
* First we need to open up Wireshark.
* Next let’s open the packet capture we downloaded. Click `File > Open` and search for the file downloaded earlier.
	* Double click the packet capture.
* You should notice  all the traffic seen is 802.11 traffic. You shouldn't be able to decipher any of the data on the window. This is why we need to use the key to see the data. 
	* Linux
		* Click `Edit > Preferences`
	* Mac
		* Click `Wireshark > Preferences`
	* You should be presented with a window similar to the one below.
	* Double click on Protocols in the panel on the left.
	* Under the Protocols section look for IEEE 802.11
	* Click on the Check box next to 'Enable decryption'.
	* Click the 'Edit…' button and a window like below will come up. Here we can add the key we retrieved using aricrack-ng 
	* Click the '+' button and you'll notice a new row is added and we can add the passcode like below. 
	* Click 'OK' for the rest of the windows and Wireshark will begin decrypting the wireless packet capture.
* Because one of our tasks is to determine the IP address of a host with DNS traffic this is where we will start.
	* In the filter section type `dns` and hit enter twice. This will apply a filter to the traffic so  we can view only DNS traffic.
	* Notice the column headings:
		* No. 
			* Represents the packet number.
		* Time
			* Time from first packet. For example, if we look at the first row Time is 11.507900. This means packet 979 was sent 11.5079 seconds after the first packet in the packet capture.
		* Source
			* This is the source IP. This is the address where the packet originated from.
		* Destination
			* This is the destination IP. This is the address where data is being sent.
		* Protocol
			* This field displays the format, if you will, of the data.
		* Length
			* This is the amount of data being sent.
		* Info
			* This is a brief summary given to us by Wireshark to quicken the analysis process. 
	* In order to view the data, we have to collapse some of the menus to see the queries and answers occur in DNS traffic. Let’s first find the IPv4 Address of fast.com. If you look at the info below on packet 979 you can see under the Domain Name System section, it shows (query). This means your computer is asking the DNS server what the IP address is for fast.com.
	* Next if we look at packet 958 we can see its similar to packet 979 but it says (response). This means the packet received is giving us the answer to our query made in packet 979. If you look below you can see it gives the:
		* Name: fast.com
		* Address: 23.11.168.39
 
This concludes the tutorial. From here on you should be able to determine a hosts IP address from DNS traffic as well as crack a password on a packet capture. 
 
### References
* http://aircrack-ng.org
* https://linux.die.net/man/1/aircrack-ng
* https://www.wireshark.org
* https://www.wireshark.org/#learnWS
* http://brew.sh

[windowskey] https://github.com/raul5660/Wireless-Password-Cracking-and-Traffic-Analysis/blob/master/image002.png "Windows Key"
