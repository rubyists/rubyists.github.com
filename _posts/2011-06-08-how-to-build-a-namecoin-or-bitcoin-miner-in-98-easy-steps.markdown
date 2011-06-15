--- 
layout: post
title: How to Build a Namecoin or Bitcoin Miner in 98 Easy Steps
author: TJ Vanderpoel
---
If you don't know what namecoin or bitcoin mining is, this article isn't for you.  See [Bitcoin Home](http://bitcoin.org) for that intro.

## Part 1 (Steps 1-6): The Hardware

For all of these steps this I _highly_ recommend using local sources.  Feel free to look online but everyone who knows anything 
about mining has already picked the cherries.   Even well-stocked stalwarts like newegg are out of all the cards that can 
handle mining 24/7 at good rates without overheating.  Find a Ryan.  Ryan is an encyclopedia of hardware who works at Fry's 
Electronics in Arlington, Texas.  I don't mean find just him, but find someone who knows their stuff.  10 Minutes with a Ryan
and you get a great rig for a great price.  The hardest to find cards are available in the bargain bin and Ryan knows where they
are.  He knows which of the newest cards can be clocked highest without overheating.  He knows all the OEM parts they only keep
in the Cage and never show pricing for otherwise (RAM for $12, Sata Drive for $20).  There's nothing (other than the mighty
Radeon) more valuable for rigging than a Ryan.
   
1. See this [Mining Comparison](https://en.bitcoin.it/wiki/Mining_hardware_comparison) and make your own determination on which ATI Radeon to get.
   
   Important factors in choosing a Radeon include mhash/s, mhash/$, and mhash/J(oule). This is going to be the majority
   of your purchase price in most miners, so don't scrimp.  If you can find a 5830 these are said to be the cheapest
   mhash/$, but the 6870 (Asus brand has been our best) is easy to find and will do ~300mhash and can be found for ~$200.  
   Such a card can pay for itself mining 24/7 for about 4-6 days.  You can find the gems/gold (5870/5970/6990) from time
   to time in small stores (Altex in Texas had a few in each store), grab them when you can.

2. Pick your Case.  

   This step is where I've made the most mistakes.  A great Name Brand means nothing if the cards won't fit.  Sometimes the
   bottom cards (in a 2-way setup) will hit the PSU, sometimes the top one will hit the CPU or some mondo fan they've included
   to look cool.  What you need is space.  Space for the 2 (or 3, or 4) gigantic radeons (seriously, go look at one) to fit in the
   thing without running in to anything important.  It also has to have a lot of airflow.  Sure you can watercool or whatever, but
   we want cheap and low-maintenance.  Put a fan in every place available, a good one (coolermaster 120mm for $9 does great here).
   Our current favorite case for rigging is the Antec DF-85.  It's around $150 but has a ton of space, no internals in the way, a
   well-located side-port for a fan to exhaust video card heat, and great airflow with the 7 included fans.  It should have a
   'backside' behind the motherboard where you can run all the cables out of the way so they don't interfere with airflow.

3. Motherboard Time.

   Space.  That's all you care about.  Look at the motherboard, personally (this is where Ryan helps again) and make sure there's
   not a sata controller under where your massive Radeon needs to extend.  Make sure it's not going to block your Power and Reset
   cables (I'm looking at you, GigaByte).  You may have to make a mistake here, yet another reason to go Local.  Make sure they
   have a return policy in case you got it wrong.  MSI 790x-g55 is our latest buy, fitting two Radeon gigantors with decent spacing.
   We have not yet tried a 3 or 4 way setup, as the PSU price and mobo price jump substantially, and the cooling seems to be hitting
   a threshold already.

4. Processor and RAM

   Who cares?  If you're using it for a miner, buy the cheapest (OEM, make Ryan bring out his little special price sheet) crap they have
   in stock, it'll never be used.  If you are using it for other than a miner, make your own decisions, just know that you don't be able
   to do anything graphics-wise while it's mining.  We have had great luck throwing a Radeon in a mail or dns or db server and having
   the GPU mine while the regular server processes are running, mining on the GPU seems to not affect the rest of the system much at all.
   If you can find a cheap CPU internal, self-contained liquid cooler like the Antec Kuhler, it could lower the overall temperature of
   the internals, which could significantly lower the temps on your Radeons.  Ymmv, the jury is still out on cost vs. temperature decrease
   on that one.  In the most recent rig Radeon temps are 4C lower with the Kuhler than with a stock CPU cooler.

5. PSU

   The power supply has to be strong enough to run your cards at max power.  I don't want to begin to think about the math here, but a 
   Coolermaster 800W Gold and Raidmax 850 have been able to run every rig we have (all dual cards, from Radeon 5850-6970).  Did I mention
   a Ryan?  You really want something that draws as little power as possible, as your electric bill is the only recurring expense of mining.

6. Extras

   As stated earlier, fill in all the fan spaces (exhaust from the side port, if possible).  Unless you're using this for something other than
   mining you don't need anything else, save your money.

    
## Part 2 (Steps 7-16) Build the Rig

Yes, you can get someone else to do this.  Maybe your Ryan does it on the side.  Maybe you know the MacGuyver of hardware.  One way or another,
someone has to build it, here's how to do it yourself.  Grab scissors or a box knife, a #2 phillips,  and a needlenose (optional, do you have tiny fingers?).

7. Open the case

   Take it out of the box, and open both sides.  take all those crazy cables running from the front panel and route them to behind the motherboard.
   If you didn't get a case where you can do this, go back to Part 1, section 2.  Get all the cardboard crap out of it and clear plastic crap off
   off everything.  Put all the hardware that came in the little bag or box somewhere easily accessible.  

8. Put the hard drive in it.

   Most cases of this class are fairly tool-less here.  You may need to put some doo-dads on the side of the drive and slide it in or (in the DF-85)
   just find out how to open the front panel and slide it in.

9. Put the motherboard faceplate in the case.
  
   If you forget this step you will really be hating yourself later.  It takes 4 seconds but it's important enough to make its' own step.  It's the
   metal rectangle with all the holes for the PS2, USB, network, and other connections are.  Just snap it into the case, removing the one
   that came with the case first, if there is one.

10. Put the Processor and Ram in the Motherboard

    Do this before you mount the motherboard to the case, yes.  You should only have to take the motherboard out of the anti-static bag and
    put it right back down on that grey foam underneath.  This way when you apply pressure to the RAM it'll be softly backed.  You also
    get the advantage of full lighting without the case in the way.  Shouldn't be anything to think about here, the processor only fits one 
    way and if you followed Part 1, Step 4, you only got one stick of cheap ass ram, just put it in a slot.  If you're using the stock CPU
    fan/heatsink, mount it now, as well.  It's a pita, involving hooking one end of the lever/hook system then hooking the other and pulling
    down a torsion lever.  If you didn't use a stock fan/heatsink the aftermarket cooler should come with its own instructions.  Mount it now
    only if it makes sense (does not for most liquid coolers).

11. Mount the Motherboard.

    Line up as many of those brass mounting open-threaded bolt/screw thingies as you can with holes in the motherboard.  6 is the minimum, more
    is better, and you can't get too many.  Generally unless you get an E-ATX board you're stuck with 6, live with it.  Line them up, squeeze
    your board _under_ the metal springy things on the backplane (you did Step 3 of this Part, right?), and put in the motherboard screws to secure
    it (motherboard screws come with the motherboard or case, and are usually labeled as such).  If they aren't labeled, find the hexagonal screws
    with coarse threads.  Do not over tighten, it only has to be snug.

12. Mount the PSU

    This is 4 screws (sometimes thumb screws) that only fit one direction.  Mount it snug but don't over tighten.  Don't put any modular power
    rails (cables) in place yet, and run all the cables out the backside as in Step 1 of Part 2, if possible.
    .
13. Hook up all the cables that do stuff.

    You probably only need Power Switch, Reset SW, and enough USB to install, but it's nice to also have all your USB, the Power LED, and
    any doo dads you want to use (audio?).  Make sure none of the optional stuff gets in the way of your Radeons, or ditch them.  Hook your drive's
    SATA cable up at this time, but not the power, yet.

14. Hook up all the cables that power stuff.

    Those thick cables you ran through the back in Step 6, Part 2, now should find their homes.  There will be an 8 prong female to plug your 8 prong
    male (or two 4 prongs on the same cable) and one 20something prong really thick one that has only one obvious female large enough to take it.  Run these
    behind the motherboard if possible to the closest opening to the female and insert them until the click.  All of those fans will need rails run to them, 
    as well, of that big 4 prong type that's a pain in the neck to get secure.  Fiddle with them until they're all plugged in.  Now you can route the SATA
    power to your single lonely hard drive.

15. Put those Radeons in.

    If all has gone well, you have room to fit the two Radeons into the two PCI-E slots on the motherboard.  They should have a gap of more than 1/2 between them,
    if not you better have a great side-port fan exhausting the heat.  They click and have 2 screws on the PCI bracket holding each secure.  Each needs a 12 prong
    (usually dual 6-prong) rail run from the PSU to it after it's mounted.  Make sure you hold the back side of the card secure while you attach the power rails, 
    for those cards which have the power lead facing the outside of the case.

16. Tidy and close it up.
  
    Take any cable ties the hardware came with and make sure you keep all the cables away from the airflow.  Behind the motherboard is always best, but some will
    have to be exposed, just tie them out of the way.  Now put the case side panels back on.

##  Part 3 (Steps 17-29) Install the Operating System.
     
This is a step-by-step of how we build archlinux manually.  A build could be scripted with the aif arch install framework, as well.  This in no way supercedes
or replaces the [Official Arch Linux Install Guide](https://wiki.archlinux.org/index.php/Official_Arch_Linux_Install_Guide) nor the 
[Beginner's Guide](https://wiki.archlinux.org/index.php/Beginners%27_Guide).  Please refer to those documents for build setups outside the scope of this simple miner.

17. Download Arch Linux

    Download 2010.05 from the [Arch Linux Download Site](http://www.archlinux.org/download) and follow the instructions to burn it to a cheap USB stick.  You
    can get 32 bit or 64, we're using the 64bit version, instructions should be the same.

18. Boot Arch Linux

    Put the USB stick you wrote the archlinux installer on in a USB slot, power on the computer, and watch for the 'Hit _ For the Boot Menu', where _ is some
    key which lets you modify the device to boot from.  Choose your USB stick and continue booting.  When you get to the screen that says "Arch Linux Live ISO",
    you're there.

19. Log In and Get it on the network.

    Log in with 'root', it will have no password.  For a simple wired connection, this can be done with 
    
        dhcpcd eth0

    If you have a wireless network,

        /arch/setup

        Now choose 1) Select Source
        Then 2) net NET (FTP/HTTP)
        Hit <OK> when it talks about loading modules
        Now 1) Setup Network
        Choose your Wireless Card (probably wlan0)
        Fill in your wireless network information.
        Say <Yes> to "Do you want to use DHCP?"

    You should see "The network is configured."

    If you have another machine on that network and want to use it for the install, you can exit the installer now and complete step 20.

        Hit <OK>
        Then 3) Return to Main Menu
        Then 8) Exit install

    Otherwise skip to Step 21.

20. Start SSHD And Log In

    Installing via ssh over the network has many advantages, the primary one being you can look at this guide split screen and copy/paste to the ssh
    session.  To start ssh

        /etc/rc.d/sshd start
        echo "sshd : ALL : allow" >> /etc/hosts.allow
        passwd
        (set a password)

    Now ssh from your regular computer to the IP of the new miner.  You can find its current IP with

        ip a l

21. Start (or continue) the Arch Linux Installer

    You should be at the Main Menu of the arch linux installer.  If you ssh'd in, this can be started with

        /arch/setup

        Choose 1) Select Source
        Choose net NET
        hit <OK>

22. Choose a Mirror
    
    Choose a mirror which is close to you geographically, if possible.  Don't use a throttled one. http://mirror.rit.edu is good in the U.S.
    Now "Return to the Main Menu"

23. Set Clock

    Select your region and timezone, then set time and date.  Always choose UTC for the Clock Configuration.  If the time is off after you select UTC,
    choose the ntp option to set the time and date using ntp.  Return to the Main Menu

24. Prepare Hard Drive

    You got the cheapest POS hard drive you could find, right?  Just choose 1) Auto-Prepare, choose the hard drive (usually /dev/sda),  and let it
    use all the defaults it presents you for partition sizes.  When it asks for Filesystem selection, we use ext4 but you're free to use any you like.
    Let it COMPLETELY ERASE! the drive you chose.  You are absolutely sure.  If for some reason this fails you can manually partition or choose the
    individual block devices yourself.  You only need / and Swap.  Back to the Main Menu when it says "Paritions successfully created"

25. Select Packages

    Choose both base and base-devel, then when the big screen with all the packages comes up find 'netcfg' and choose it, then hit <OK>

26. Install Packages

    Hit <OK> and this runs on its own.  Watch and wait til it's complete.

27. Configure System

    Choose this, say Yes to using network settings from the installer in rc.conf and resolv.conf, then choose an editor.  If you don't know any of them,
    choose nano.

        /etc/rc.conf: Change the HOSTNAME to something you wish to name this.  
        Now find the line that says #NETWORKS=(main) and remove the # from the beginning of that line
        change DAEMONS=, replacing 'network' with 'net-profiles' and adding 'sshd' (no quotes on any of them)
        Save the file

        Skip /etc/fstab and /etc/mkinitcpio.conf
        /etc/modprobe.d/modprobe.conf: add a line that reads 'blacklist radeon' (without the quotes).  Save the file

        Skip til /etc/hosts.allow: add 'sshd : ALL : allow' to it and save it.

        Skip to Root-Password and set one.  Make sure you get it right, it won't give you much warning if you get it wrong.
        Choose Done, it will do a little scroll dance after a few seconds then return to the main menu.

28. Install Bootloader

    Choose Grub, then hit <OK> to open an editor for the grub configuration file.  Find the first line that says kernel /vmlinuz26 root=... and add
    'nomodeset' to the end of it (no quotes).  Save the file, then choose the correct device to install the bootloader on, likely /dev/sda.  You should
    see "GRUB was successfully installed".

29. Exit install and reboot.

    Once you exit the installer, type reboot -f and hit enter to reboot.

## Part 4 (steps 30-61) Install the miner software.

30. Boot the new Arch Linux and Log In.
 
    It should boot on its own, all the way to a log in prompt.  Log in as root with the password you set in the "Configure System" step.

31. Set up the network.

        cd /etc/network.d
        cp examples/ethernet-dhcp ./main
        netcfg main

    If you aren't using ethernet-dhcp, or have to choose a different interface from eth0, you can edit the file and change any parameters you need.
    In /etc/network.d/examples are other types of connectioned clearly named.  Copy any one of them you need to /etc/network.d as 'main' and edit
    to taste, if the above did not work for your connection.

32. Create a normal user account

    This is the user that will run the mining and you can also use it to administrate the system.  If you want separate admin account from the miner
    account you can create as many as you like here.

        useradd miner

    Follow the prompts, accepting defaults if you don't want admin privileges for the user, adding the group 'wheel' (no quotes) as an 
    Additional group when prompted for Additional groups.  

        New account will be created as follows:

        ---------------------------------------
        Login name.......:  miner
        UID..............:  [ Next available ]
        Initial group....:  users
        Additional groups:  wheel
        Home directory...:  /home/miner
        Shell............:  /bin/bash
        Expiry date......:  [ Never ]

    If yours looks like that, hit ENTER to create the account.  You will have to set a password, you don't have to fill in the other information, 
    you can just enter through it all, leaving them blank.

33. Install some software

        pacman -Syu

    This will make sure everything on your system is current.  Pay attention to anything it tells you to do while it's upgrading.  You will likely
    have to 
        
        pacman-db-upgrade

    Before it will allow you to use it

        pacman -S rsync git openssh yajl

    Once this installs, you can start openssh with /etc/rc.d/sshd start and log in remotely to continue the steps, if you prefer.

        cd /tmp/
        wget http://aur.archlinux.org/packages/package-query/package-query.tar.gz
        wget http://aur.archlinux.org/packages/yaourt/yaourt.tar.gz

