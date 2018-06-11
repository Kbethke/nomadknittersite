---
title: "Major Electronics Upgrade!"
date: 2017-08-14T10:00:00
draft: false
tags: ["life"]
categories: "sailing"
---

{{% figure src="/2017/OpenCPNMacOSX-840x560.png" %}}

Anchored off a remote island in the Venezuelan archipelago of Los Roques it was magical to lay back on the flybridge and watch the dark and clear starry night. I used the word “watch” over the expected “star-gaze”, because with the night sky so clear and dark, there were plenty of meteors burning through the sky about every few minutes, demanding a much more active viewing than gazing.

Sitting up there sipping some aged island rum with solar created ice is such a serene experience. But after reaching maximum chill state, my fingers creep over and grab my laptop. We are 10 miles away from the big town of 1000 people of Gran Roque, 90 miles north of Caracas, with zero internet access here, and zero mobile access. Yet, I am connected to the 5 GHz band on the Ad Astra wifi net. Why?

Flipping the laptop open, I start scanning our cloud drive that has 1.2TB of movies and serials. Watching <your favorite movie> with the stars as the theatre backdrop and icy rum in my right hand…

I have just finished a major upgrade of the electronics capabilities of Ad Astra and I wanted to share what I have installed and my learnings.

### Newly Added Systems:

* [VesperMaine AIS Transponder](https://www2.vespermarine.com/xb8000-ais-transponder)
* [DigitalYacht’s iKommunicate marine system multiplexer & data server](https://ikommunicate.com/description/)
* [Western Digital’s 4 TB Dual-drive MyCloud network storage](https://www.wdc.com/products/personal-cloud-storage/my-cloud.html#WDBCTL0040HWT-NESN)
* [Linksys’ Smart Wifi Router](https://www.linksys.com/vn/p/P-EA6350/)
* OpenCPN on MacOSx, iOS and Android (with worldwide charts)

### Existing Systems:

* Raymarine E120W chart plotter and ST70 MFD displays
* Raymarine GPS, Wind, Depth, STW, and auto pilot
* [BadBoy eXtreme WIFI antennae & hotspot server](http://www.bitstorm.com/)

This past spring I met up with my friend Peter Wraa – a most excellent Danish sailing captain and former stunt pilot – in the off the beaten track [Petite Terre islands](https://en.wikipedia.org/wiki/Petite_Terre_Islands). Naturally, we started talking about what we want to upgrade to our boats. Peter was satisfied with our big 2000 watt solar update looking like it fit in with the existing Lagoon 450 fit and feel (a big compliment from Peter as he is fussy and thinks our fully enclosed flybridge looks like a doghouse) and started to tell me about his upgrades.

Specifically, he has been replacing his electronics and moved to a [NMEA 2000](https://en.wikipedia.org/wiki/NMEA_2000) (think ethernet for marine systems) network backbone and showed me that he could see all of his ships’ instruments from his iPhone. Definitely cool, but at that moment I just wanted to snorkel the pristine waters were I saw over 70 big lobsters in a single snorkel with Max.

Despite working in the games industry and being a programmer, when it comes to tasks like programming the VCR, or getting the most out of the electronic systems on the boats I have owned, I never felt a great motivation. I got enough tech in my day job. Mostly, I want to enjoy the water. And if I am going to be in tech mode, I would like to save my brain’s bandwidth for working on a software project.

Nevertheless, Peter would not stop. He kept going on and on. He thrust a decade old NMEA 183 multiplexer / data server into my hands. Promising me that it would splice into a proprietary Raymarine cable and would then be able to transmit the Raymarine ship instrument data across ethernet to our smart phones and laptops. It had 18 gauge power and data leads and its IP address and port scrawled in faint pencil on the label of the device. Clearly, the fastest path to me being able to enjoy the snorkeling was to accept the gift from Peter and promise him that I would install it.

My main focus in May was to complete the coursework and testing for my US Coast Guard Masters’ License, the NMEA 183 was safely tucked away in a drawer. In early June, I spent a week in Miami taking the captain’s test as well as all the associated overhead tasks. But while I was there, I made mad use of Amazon and fast & cheap delivery to my hotel room. The [Western Digital MyCloud dual-drive 4 TB network storage device](https://www.wdc.com/products/personal-cloud-storage/my-cloud.html#WDBCTL0040HWT-NESN) was one of those packages. The other was a high-speed network switch.

On Ad Astra we have about a dozen portable USB drives of various vintages and capacities that we trade around and try to remember which one has what data. We also use those to backup our Macs. It is pretty confusing. Sure, we should label them more clearly and have a more ordered use pattern. But an always on, single network drive would be much more elegant.

The first project was the integration of the WD network drive with the Ad Astra WIFI hotspot.

The existing [BadBoy eXtreme is a WIFI](http://www.bitstorm.com/) antennae with the optional WIFI hotspot accessory that connects any number of your devices to an external WIFI hotspot. Throughout the Caribbean there are internet providers that cater to cruisers such as Pirate WIFI or Captain’s WIFI by pumping out a WIFI signal that you can pickup on your boat and pay them for the day, week or month. Almost all marinas also have some WIFI that your boat pays for when in the marina, St. Barts has a municipal WIFI, and many bars and restaurants provide WIFI in exchange for some patronage. The BadBoy eXtreme works by creating a WIFI hotspot inside your boat where all of your devices can easily connect and have strong signal. Then it takes that traffic up the mast to your antennae with powered ethernet (15v!) it sends out your WIFI traffic with significantly boosted signal strength. In fact, in many anchorages you need to turn down the power of your antennae for better results so as to minimize noise between the various boats.

### More Ports Please!

We have used the BadyBoy system for a year and were very happy with this boat purchase, but the WD network drive has a hard ethernet cable and needed to be plugged into a router or switch. The BadBoy eXtreme naturally to keep costs down has a single ethernet port – designed for the internal boat-facing WIFI hotspot itself. I needed more ports. To address this need, I purchased a [Linksys EA6350 Smart WIFI router](https://www.linksys.com/vn/p/P-EA6350/) that had five ethernet ports.

Here is how I used the ports on the new router:

1. inbound WIFI from the [BadBoy](http://www.bitstorm.com/)
2. WD network drive
3. iKommunicate multiplexer and data server
4. direct ethernet connection of a laptop for higher-speed transfers
5. spare port

{{% figure src="/2017/LinkSys1-768x556.png" %}}

The Linksys has a nice admin panel that is available at the default address of 192.168.1.1 from there you can create both 2.4 and 5.0 GHz networks, name them whatever you like, and give each network separate passwords, and many granular controls.

{{% figure src="/2017/LinkSys2-768x609.png" %}}

One that I found most useful was the ability to set reserved IP addresses for specific devices on the DHCPaddress server. The [VesperMaine AIS Transponder](https://www2.vespermarine.com/xb8000-ais-transponder) and the WD network drive tended to wander around in IP address space, and this allowed me to lock them down so that I did not need to update the IP address on the apps for these devices every time I powered up and down the devices.

{{% figure src="/2017/WDHD1-201x300.png" %}}

### Details on the WD network drive:

The WD drive comes with RAID 0, RAID 1, JBOD, and Spanning as options for using the capacity of the two drives. Basically JBOD lets you just manage the two disks as if they were two hard drives. Spanning logically bridges the two drives to create one larger drive, RAID 0 stripes your data across two drives and allows faster read & writes, and RAID 1 is the most conservative with mirroring on both drives allowing continued access to the data even in the event of a drive failure.

{{% figure src="/2017/RAIDMode-768x464.png" %}}

I set it to RAID 1 and as of this moment, I am actually using the RAID re-build feature. There is some sort of failure going on with the WD drive. The symptoms were spotty drive reading over the last 24 hours, and last night the public section of the drive refused to show its contents – even though the data was there. On the impressive web-base control panel for the WD unit I was able to direct the RAID to rebuild the drive, and in the future auto re-build in case of new fail events. At the moment I do not know if we are experiencing a hardware failure, or perhaps one of the two recent guest users (on PCs) maybe have infected the cloud drive with a virus. After the rebuild cycle, all the data is there and the drive is behaving well. This makes me a fan of RAID 1, but there is the open question of where this failure came from in the first 60 days of use on Ad Astra. The WD unit is modular in its design and facilitates rapidly popping in and out new drives (even from other manufacturers) and so I will have Kaiwen pick up a couple of extra drives when she goes back to Austin in October to further the strong redundancy and spare capabilities of Ad Astra.

{{% figure src="/2017/RebuildingRaid-768x481.png" %}}

[Edit: one week later the drive is stable after the re-build, but the original folder that hosted the movie data remained unable to be seen as a network drive by the Macs. Using the web-based WD utility I was able to create a new folder and move the data over – and it has been working fine.]

The WD unit has the ability to create users and user groups, and create individual share volumes that have a ton of settings such as giving users storage quota and access to specific shared volumes. With sensible use of users and permissions I will be able to isolate away any further virus infections from guests.

### Tossing away the Power Bricks

Now, a super cool thing about both the Linksys WIFI router and the WD network drive is that the both had a AC to DC power brick that converted 110 VAC to 12 VDC – Woot! Same 12 VDC as the house bank for Ad Astra. We are now able to stream movies to four different laptops all night long over WIFI without using an inverter! This also means that when the main 110V inverter is running that we are not taking 12 VDC from the house bank and inverting to 110 VAC and then pouring that back through a dumb power brick to 12 VDC and needlessly wasting power by warming up some wires. (I lost some dollars on the first network switch that had the AC to DC converter sleekly integrated directly into its circuit board – preventing it being powered by the 12V house system.)

To connect these to the house power, I sourced identical power barrel plugs at a local electronics store in SXM (I kept the original power brick cords intact, just in case I ever wanted to use them on 110 VAC in the future). With the barrel plugs I wired the devices to the new 6 circuit breaker panel that I needed to setup up to power the VesperMarine AIS, the VHF antennae splitter, and iKomminicate multiplexer, to round out the sixth circuit I re-wired the power for the BadBoy to this breaker panel. I added a nice 6 circuit fuse panel and mounted all of this discretely under the navigation table. Now I have the ability to individually power each of these systems:

{{% figure src="/2017/Screen-Shot-2017-08-14-at-3.04.47-PM-731x1024.png" %}}

1. BadBoy – perhaps we are away from a place with WIFI and want to save 1-2 AHs
2. VHF Splitter
3. AIS – perhaps we are someplace where we do NOT want to broadcast our position Venezuela?
4. The iKommunicate Multiplexer – normally we have the Raymarine instruments off when not moving, so no use powering a multiplexer on a network that is not running traffic
5. WD Network Drive – to save again some small AHs
6. Linksys WIFI router – to save again some small AHs

### Can you See Me? Please!?

The next new piece of electronics is the [VesperMaine AIS](https://www2.vespermarine.com/xb8000-ais-transponder) recommended to me by Renee at 
Island Water World in SXM.

{{% figure src="/2017/connection.png" %}}

AIS broadcasts the position, heading, and speed the boats and ships around you typically out to 8 or 10 nm.

AIS is probably up in the top 3 of safety equipment in my mind when at night and in poor visibility and the lights and movement of the other ship is not very clear. With AIS it tells you directly your time to closest approach and how far away will you be at that time. All ships that are over 60 feet in length are required to run AIS in most territories these days. However, smaller boats especially smaller fishing and sailing boats often choose not to spend the $1000 to have a transponder installed. Very understandable, but for me, it is even more important to me that the other [bigger] boats can see me on AIS, than if I can see them. I have total control over how well we keep watch, but I cannot control the watch-keeping on the other boat. It is pretty easy to forget to look at the actual sea and instead glance at the AIS screen and the GPS chart plotter.

The VesperMarine AIS transponder is made in New Zealand and is very cool, it too is a WIFI hotspot capable of broadcasting the AIS and GPS data that it is producing from its own dedicated GPS receiver as well as it’s split into the VHF antennae.

{{% figure src="/2017/AIS0-576x1024.png" %}}

### The [VesperMaine AIS](https://www2.vespermarine.com/xb8000-ais-transponder) installation was very straightforward:

1. Wire it up to 12 VDC
2. Install the GPS outside with a clear view of the sky
3. Drill a hole for the cable to pass through and use a waterproof collar for that wire to enter the fiberglass.
4. Connect the GPS cable to the VesperMarine
5. Attach the VHF splitter and run one branch to the VesperMarine and one to the existing Raymarine bus.
6. Connect the VesperMarine to your NMEA 2000 bus
7. Use the USB connection to your laptop and configure the device

Using the vmAIS for MacOSX, and connected to the VesperMarine unit though a direct USB cable, it took only a few minutes to enter our MMSI and boat information, tell it to stop being its on WIFI hotspot and instead give it the password to the Ad Astra 2.4 GHz crew network. After getting assigned its IP address from the Linksys router I used the advanced networking options on the Linksys admin panel to reserve the assigned IP to the VesperMarine unit.

### The WatchMate Android app has four useful views in your pocket:

An AIS Radar display that uses two finger zoom from 0.25 to 50 nm!

{{% figure src="/2017/AIS1-576x1024.png" %}}

A list view of all AIS targets sortable on priority, time to closest approach, closest approach and range

{{% figure src="/2017/AIS3-576x1024.png" %}}

Core navigational data: GPS Latitude and Longitude, Course over Ground, Speed over Ground, Heading, Depth, Apparent Wind Angle and Apparent Wind Speed.

{{% figure src="/2017/AIS2-576x1024.png" %}}

The VesperMarine is picking up the Depth, AWA and AWS from the NMEA 2000 network, although AWA and AWS are not rendering. Which is weird. The RayMarine i70 happily displays AWS but not AWA. And to my surprise, the [iKommunicate multiplexer](https://ikommunicate.com/description/) is outputting very accurate AWA and TWS on the sample iCompass map. So, my wind instrument works – both AWA and TWS, but I still need to figure out what the heck is going with this wind instrument’s flaky rendering. Hmm, the Boat List is never Zero.

### iKommunicate

{{% figure src="/2017/iKommunicate-Top-Angle-HR.jpg" caption="The  iKommunicate Multiplexer" %}}

Installing the iKommunicate Multiplexer was very straightforward:

1. Wire up the red and black wires to 12 VDC
2. Connect a RayMarine to NMEA2000 cross over cable into one of the blue ports on the raymarine network and then connect to the NMEA2000 port on the iKommunicate
3. Connect an ethernet cable to your router or switch
And that is it really!

Now you can connect any [Signal K application](http://signalk.org/) from any device to all of your ships’ systems!

Below is a screenshot of [OpenCPN](https://opencpn.org/) displaying where we are anchored off of Bonaire with AIS targets visible.

{{% figure src="/2017/OpenCPNMacOSX.png" %}}

Now, *every* tablet, smartphone and laptop on Ad Astra (some dozen devices) is now marine multi-function displays!

I am just scratching the surface of SignalK and the iKommunicate device.

This is an open source device with a [github](https://github.com/digitalyacht/ikommunicate) for extending the software as you like!

I cannot wait to dig into the [wiki](https://github.com/digitalyacht/ikommunicate/wiki/iKommunicate-Developer's-Guide-(SDK)) for this device and start to customize the digital bridge of Ad Astra!

{{% figure src="/2017/AndroidOpenCPN-576x1024.png" %}}