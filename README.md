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
