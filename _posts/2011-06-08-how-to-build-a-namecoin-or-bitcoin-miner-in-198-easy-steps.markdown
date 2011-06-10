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
   mhash/$, but the 6870 (Asus brand has been our best) is easy to find and will do ~300mhash and can be cound for ~$200.  
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

9. Put the motherboard backplane in the case.
  
   If you forget this step you will really be hating yourself later.  It takes 4 seconds but it's important enough to make its' own step.

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
  
    Take any cable ties the hardware came with and make sure you keep all the cables away from the airflow.  Behing the motherboard is always best, but some will
    have to be exposed, just tie them out of the way.  Now put the case side panels back on.

##  Part 3 (Steps 17-51) Install the Miner Software.
     
