---
layout: post
title: Charging AirPods with an iPhone
comments: true
---
<meta property="og:image" content="https://raw.githubusercontent.com/sunnybala/sunnybala.github.io/master/assets/solder_lightning.jpg"/>

Several [tech journalists](https://appleinsider.com/articles/19/04/01/2019-iphone-should-be-able-to-power-the-airpods-wireless-charging-case) have speculated that the new iPhone later this year will be able to share its power to charge AirPods. That's great for people that buy the new airpods and the newest phone...but what about the rest of us? **Good news -- there's no need to wait till October 2019 when we can build something to do it today!**

<img height="400" src="https://raw.githubusercontent.com/sunnybala/sunnybala.github.io/master/assets/solder_lightning.jpg" class="center"/>

<style>
.center {
    display: block;
    margin-left: auto;
    margin-right: auto;
}

img height="400" {
	height: 250 px;
}

</style>

## The Plan

Build a cable to extract power from an iPhone and use it to charge other devices. I don't have much experience with electronics so it's also an 

## Background

I couldn't seem to find much online about this except other people asking if this sort of thing was possible. There was a [crowdfunded product](https://www.indiegogo.com/projects/chargebite-a-social-charger "Indiegogo") for something in the space 5-6 years ago, but that required having 2 iPhones to donate power to charge 1. There's one <a href="#sources"> YouTube video </a> that claimed to do this but many of the comments suggested that it was fake. Based on what I've seen, the specific claims of iPhone to iPhone charging appear to be fake but using that method for airpods works. The lack of other prior work on charging in this way, even for hobby projects, definitely made me suspicious that there was a big roadblock somewhere in the process that I would hit. I was pretty surprised to see this actually work! I think I lucked out due to the lower charging requirements of the AirPods relative to the iPhone. 

For anyone following along to try the same, **definitely proceed at your own risk with making this. I’m no expert so I can’t speak to whether this can damage your devices.**

## Hardware Setup

<u>Recipe:</u>
- [1x Lightning Port Fan](https://www.amazon.com/gp/product/B078P7WVKD "Amazon")
- [3V to 5V Step Up Board](https://www.amazon.com/gp/product/B07F266X24 "Amazon")
- 1x Lightning cable
- Some wire

I bought a 4 pack of the fans knowing I'd probably mess up and break a few. That ended up being a good call because I broke 3. 

<u>Required Tools:</u>
- Soldering Iron
- Some sort of cutting tool (I used a Dremel)

I also found my hot glue gun and heat shrink tubing for wires helped keep things in place but it's not necessary.

## Build

 There’s only 2 steps here to get this working.

#### Step 1: Power Extraction

The iPhone lightning port isn’t really meant to be used to draw power. Finding information on how to do this is pretty tough – perhaps being a member of the Apple hardware development program would make finding that easier, but only corporations can join (not individuals). Finally, lightning devices have authentication chips inside the cable, so building this part from scratch didn’t seem viable for me.

However, there are lightning accessories that don’t have power of their own. For example, [this keyboard](https://www.amazon.com/Logitech-Wired-Keyboard-Lightning-Connector/dp/B079VTRQVG) or [this flash drive](https://www.amazon.com/SanDisk-iXpand-Flash-Drive-iPhone/dp/B01CIEBU22) or [this fan](https://www.amazon.com/Pack-Phone-Fans-Portable-iPhone/dp/B078P7WVKD). If we can just take apart one of these accessories, we can take advantage of whatever power drawing method they’ve built. I chose this 1 Watt lightning fan based on something I saw in a <a href="#sources"> similar YouTube video</a>.

I used my Dremel to cut away the hard plastic casing of the fan. Despite being lightning, the fan only worked when plugged into my phone with the fan facing me. When facing the fan away from me, nothing happened. While taking this apart, make sure to remember which way the cable has to be plugged in. For me, the flatter side of the chip had to be facing up.

<img height="400" src="https://raw.githubusercontent.com/sunnybala/sunnybala.github.io/master/assets/fan_cut.jpg" class="center"/>

Next, I de-solder the connections between the chip and the fan’s motor. This gives me a standalone chip. Plugging it into my phone and using a multimeter, I recorded a voltage of 3.3V. 

<img height="400" src="https://raw.githubusercontent.com/sunnybala/sunnybala.github.io/master/assets/de_solder.jpg" class="center"/>

#### Step 2: Matching Voltage

This presents 2 issues. USB charging needs 5V, so we’ll need to step it up. However, once we step it up, we’ll only be running 200 mA (1W/5V) through our charger. For comparison, standard Apple wall USB chargers are 5 Watts and therefore provide 1000 mA (and 2000 mA for fast charging 10 Watt adapter!). This 200 mA is too low for charging Apple’s bigger devices (iPhone, iPad) but as I see later, it’s enough for AirPods.

I used a step up regulator I bought from Amazon to convert from 3.3V to 5V. The additional benefit is that the USB output keeps things pretty flexible on what I want to power (ex. my kindle instead of AirPods). After soldering the connection I checked my multimeter to confirm I was getting 5V. I was ready to test!

<img height="400" src="https://raw.githubusercontent.com/sunnybala/sunnybala.github.io/master/assets/solder_lightning.jpg" class="center"/>

#### Results

Plugging the devices into each other was the scary part...but it worked. The AirPods charged without a fuss and my phone didn't burst into flames. I kept track of the charge reported and measured a charge of rate of 2% per minute on the case (measured minutely over 40 minutes). My expectation was that this DIY charger would be slower. Surprisingly, that's the same charge rate that I record with the standard wall adapter over a similar measurement period! 

<iframe width="560" height="315" class="center" src="https://www.youtube.com/embed/tDveHRmT92o" frameborder="0" allow="autoplay; encrypted-media" allowfullscreen></iframe>
<br>

It's also true that if I plug my iPhone 6 into my iPhone X (using the 6 as the charge donor) the charging does appear to activate, however, it doesn't seem to actually charge. This is likely because there just isn't enough power coming out of the donor iPhone. 

## Gotchas

The iPhone fan connector is pretty sensitive and easy to break – I broke my first version of this after testing for a day just from repeated insertion. I suggest putting some sort of casing around the chip so it doesn’t flex against the lightning connector. In my final build, I cut the fan casing to keep part of the shell around the lightning cable and used hot glue to keep it fixed to the circuit. That way I could insert the cable without worrying as much about breaking the connector.

 
## Sources

<div id="sources">
<a href="https://www.youtube.com/watch?v=UTcHMXF6P9M"> This youtube video </a> claims to make an iPhone to iPhone charger in a similar way. Several comments suggest the charging shown here might be fake. From the calculations, I just don’t see how it’s possible to extract enough power to charge a phone in this way. But it’s not totally fake – I think if he had tried other devices it might have worked, just like what I'm seeing in these results. 
<div>