--- 
layout: post
title: How to Build a Namecoin or Bitcoin Miner in 198 Easy Steps
author: TJ Vanderpoel
---
If you don't know what namecoin or bitcoin mining is, this article isn't for you.  See [Bitcoin Home](http://bitcoin.org) for that intro.

## Part 1 (Steps 1-18): The Hardware

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
   well-located side-port for a fan to exhaust video card heat, and great airflow with the 7 included fans.

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

 

    

