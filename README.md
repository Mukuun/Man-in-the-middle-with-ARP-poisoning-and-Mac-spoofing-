# Man-in-the-middle-with-ARP-poisoning-and-Mac-spoofing-


This project aims to investigate and demonstrate how ARP poisoning and MAC
spoofing techniques can be leveraged to create a man-in-the-middle (MITM) attack,
enabling unauthorized traffic interception and manipulation by circumventing
router-level blocking mechanisms. This project focuses on understanding ARP
protocol vulnerabilities, exploring practical MITM attack methods, and assessing
potential mitigation strategies for improved network security.

INTRODUCTION:

MAC Spoofing:
MAC Spoofing changes the MAC (Media Access Control) address on a device’s
network interface to appear as a different device. This can be used for security
testing, bypassing network restrictions, or mimicking another device on the network.
MAC addresses are unique identifiers for each network device, but they can be
altered using software tools. Spoofing your MAC address on Parrot OS is
straightforward and can be done through the terminal or with tools like macchanger.

ARP Poisoning:
In this project, we dive into the dynamics and potential risks of network security,
specifically focusing on Man-in-the-Middle (MITM) attacks executed through
ARP (Address Resolution Protocol) poisoning and MAC (Media Access
Control) spoofing. The goal is to understand and exploit vulnerabilities in ARP, a
foundational protocol used to map IP addresses to MAC addresses on local
networks, to demonstrate how attackers can intercept, manipulate, or reroute
network traffic.

Understanding ARP and ARP Poisoning

The Address Resolution Protocol is a key component in network communications,
enabling devices to locate and connect with each other by translating network layer
addresses (IP) to link layer addresses (MAC). This translation facilitates seamless
communication across networks. However, ARP has a major flaw: it was designed
without built-in security, making it susceptible to ARP spoofing or poisoning
attacks. In an ARP poisoning attack, the attacker sends falsified ARP messages onto
the network. These messages trick network devices into associating the attacker’s
MAC address with a legitimate IP address, thus routing the device’s traffic through
the attacker’s machine rather than directly to its intended destination.
Executing MITM Attacks Using ARP Poisoning and MAC Spoofing
In this project, we utilize Ettercap, a tool designed for network security
assessments, to demonstrate a MITM attack through ARP poisoning. Using Ettercap,
we simulate an environment where an attacker can intercept, alter, or drop data
packets between devices. By sending fake ARP responses, the attacker effectively
positions themselves between two or more devices, observing or modifying the data
in real time. Alongside ARP poisoning, MAC spoofing enables the attacker to
disguise their machine’s identity, further enhancing their access to restricted network
areas.

Significance of the Project

The project highlights both the technical process and the implications of MITM
attacks. MITM via ARP poisoning underscores the critical need for network
administrators to implement protective measures, such as static ARP tables, network
segmentation, and secure routing protocols. As a result, this project emphasizes not
only the importance of understanding network protocols like ARP but also the
potential risks if network traffic isn’t properly secured.
This hands-on exploration with Ettercap allows us to analyze network vulnerabilities
and understand the measures needed to mitigate them, making it valuable for both network administrators and cybersecurity professionals interested in strengthening
network security frameworks.

PROCEDURE:

MAC Spoofing:
1. Identify the Current MAC Address
● First, determine your current MAC address by running:
ifconfig
● Look for your network interface (typically wlan0 for Wi-Fi or eth0 for
Ethernet), and note the MAC address listed under ether
2. Disable the Network Interface
● Before changing the MAC address, the network interface must be disabled
temporarily:
sudo ifconfig [interface] down
● Replace [interface] with your network interface name (e.g., wlan0 or eth0).
3. Change the MAC Address with macchanger
To spoof the MAC address, you can use macchanger. For example:
● Random MAC Address: Generates a completely random MAC.
sudo macchanger -r [interface]
● Specific MAC Address: Manually specify a MAC to mimic a specific device.
sudo macchanger -m [desired MAC] [interface]
Replace [desired MAC] with the MAC you want to mimic.
● You should see output confirming the MAC address change, showing the
original and new MAC addresses.
4. Re-enable the Network Interface
● Once the MAC address is changed, bring the network interface back up:
sudo ifconfig [interface] up
● Confirm the change by running ifconfig again and checking the new MAC
address.
5. Verify the MAC Change
● To ensure the spoofing is successful, recheck the MAC address by running:
ifconfig
● Additionally, on the network, you can verify with:
arp -a
● This displays devices connected to the same network, showing IP-to-MAC
address mappings and verifying your spoofed MAC if necessary.
6. Restoring the Original MAC Address
● To revert to the original MAC address, use macchanger with the following
command:
sudo macchanger -p [interface]
● Alternatively, simply restart the network interface or reboot your device, as
many systems default to the original MAC upon restart.
ARP Poisoning:
1. Connect the Attacking Device to the Network
● Ensure that your attacking device is connected to the same network as the
target devices.
● Identify your network interface, as you will need this for Ettercap. Use code:
Ifconfig or ip addr to identify your network details.
2. Open Ettercap on the Attacking Device
● Launch Ettercap on your device with administrative privileges. Open the
terminal and enter: sudo ettercap -G
● The -G option opens the Ettercap graphical interface for ease of use.
3. Perform a Network Scan
● Inside Ettercap, go to "Sniff" > "Unified Sniffing" and select the network
interface connected to the network.
● Then, navigate to "Hosts" > "Scan for Hosts". This will scan for all devices
on the local network, identifying their IP addresses and MAC addresses.
4. Identify and Add Target Hosts
● After the scan, go to "Hosts" > "Host List" to view all detected devices on the
network.
● Select the device you want to make the victim (usually another user’s device)
as Target 1.
● Set the gateway (typically the router or access point) as Target 2.
Note: By targeting the gateway, you will direct the victim’s traffic
through the attacking machine.
5. Initiate the ARP Poisoning Attack (MITM)
● Go to "Mitm" > "ARP Poisoning". Select the option "Sniff remote
connections" if available, as it allows capturing data transferred between the
victim and other devices on the network.
● Click OK to start the attack. Ettercap will now begin spoofing ARP responses
to convince the victim and the gateway that the attacker’s MAC address is
associated with the other’s IP.
6. Monitor ARP Packets in Wireshark
● Open Wireshark on the attacking machine. Select the same network interface
as in Ettercap.
● Apply a filter to see only ARP packets by entering arp in the filter bar and
pressing Enter.
● Observe the ARP packets for both the gateway and victim devices. You
should see that the ARP table of the victim and gateway is now associating
the attacker’s MAC address with the other’s IP address.
7. Analyze the ARP Table Before and After the Attack
● On the victim device and gateway, observe the ARP table by entering the
following command in a terminal or command prompt:
arp -a
● Note the IP-to-MAC address mappings before and during the attack. You
should see that the attacker’s MAC address is associated with either the
gateway’s or victim’s IP, indicating successful ARP poisoning.
8. Send Ping Requests to Test Visibility of ICMP Packets
● On the victim device, send ping requests to the gateway to observe ICMP
traffic. In a terminal or command prompt, use:
ping [gateway IP]
● Return to Wireshark on the attacker’s machine and apply the filter:
icmp
● Check if you can see the ICMP (ping) packets sent from the victim to the
gateway, as well as responses from the gateway to the victim. This verifies
the success of the MITM attack, as you’re now able to intercept these packets.
9. View HTTP/Browser Traffic (Optional)
● If the victim device accesses a website (HTTP only, as HTTPS traffic will be
encrypted), the attacker should be able to view some HTTP packets in
Wireshark.
● Apply the filter for HTTP packets to view this:
http
● You might see details such as the destination URLs and, in some cases,
unencrypted data being transmitted.
RESULTS :
MAC SPOOFING:

![image](https://github.com/user-attachments/assets/3eb0f3dc-e62e-4578-84a5-1310cf89efa6)

![image](https://github.com/user-attachments/assets/12e600ed-f5a4-4d15-a4e6-c6c7a72ace23)

![image](https://github.com/user-attachments/assets/238d7785-1c8b-4662-8732-2e239489acb2)

![image](https://github.com/user-attachments/assets/0372d2b9-215c-4803-b6c8-ba570f65c199)

![image](https://github.com/user-attachments/assets/530e2b92-e66e-4acf-b2a8-4592c75e781b)

![image](https://github.com/user-attachments/assets/7752b13b-5dbf-46c7-bdbc-3b26a1a383cd)

![image](https://github.com/user-attachments/assets/7cd227b1-b280-493c-a407-798f478488d6)

![image](https://github.com/user-attachments/assets/facb87e7-8373-4353-87c5-25b11e7f1719)

![image](https://github.com/user-attachments/assets/73c624e0-bf99-41f3-835b-cb8a7c9ec52f)

![image](https://github.com/user-attachments/assets/05c2f8a4-7dda-42c8-a4ac-d6cc4086635b)

![image](https://github.com/user-attachments/assets/86728c21-9427-4c0f-bec0-246a4b72925f)

![image](https://github.com/user-attachments/assets/19b1ca59-80e6-4cb4-b701-668dafbc5d18)

![image](https://github.com/user-attachments/assets/956ed58c-6b85-4e97-a668-2004b2e94014)

REFERENCES:
MAC Spoofing:
1. Ali, H.M. and Abbas, A.M., 2014. New Approach in Detection MAC
Spoofing in a WiFi LAN. Journal of Engineering, 20(08), pp.142-155.
2. Cebula, S.L., Ahmad, A., Wahsheh, L.A., Graham, J.M., DeLoatch, S.L. and
Williams, A.T., 2011, April. How secure is WiFi MAC layer in comparison
with IPsec for classified environments?. In Proceedings of the 14th
Communications and Networking Symposium (pp. 109-116).
ARP Poisoning:
1. D. R. Rupal, D. Satasiya, H. Kumar and A. Agrawal, "Detection and
prevention of ARP poisoning in dynamic IP configuration," 2016 IEEE
International Conference on Recent Trends in Electronics, Information &
Communication Technology (RTEICT), Bangalore, India, 2016, pp. 1240-
1244, doi: 10.1109/RTEICT.2016.7808030. keywords: {IP
networks;Protocols;Authentication;Servers;Internet;Local area
networks;Conferences;ASP
Poisoning;MITM;SPOF;DOS;Gratuitous;Session
hijacking;interruption;ICMP;Radius}
2. Khurana, S., 2015. A security approach to prevent ARP poisoning and
defensive tools. International Journal of Computer and Communication
System Engineering, 2(3), pp.431-437.






