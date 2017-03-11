---
title: The Braaaain 1.0
author: antitree
type: post
date: 2013-10-27T22:29:36+00:00
url: /2013/10/27/the-braaaain-1-0/
categories:
  - Hardware
  - lulz

---
Halloween time again. Last year I tried to do a simple little hardware project to make my emoticon pumpkins glow. That&#8217;s cute and all but not very difficult.

This year, I decided to work on this idea I&#8217;ve had for more than a year and a half. **The Brain &#8211; a silicone based brain with controllable LEDs inside. **I have some ideas of what to do next with it, but this first iteration is just to be a fun decoration for Halloween.

[<img class="aligncenter size-full wp-image-683" alt="Brain Mold" src="http://www.antitree.com/wp-content/uploads/2013/10/2013-10-27.jpg" width="486" height="648" />][1][<img class="aligncenter size-full wp-image-686" alt="Glow Brain" src="http://www.antitree.com/wp-content/uploads/2013/10/2013-10-27-1.jpg" width="486" height="648" />][2]

&nbsp;



# Why The Brain

Back when Interlock was moving into their current space, there was this cool area in the center of it, that was surrounded by windows. That turned into the network room and I really wanted to use that window for something. Show something cool in the window or whatever. I came up with this idea that I would have a brain to represent network activity. When a host goes down, the brain reflects that. If there was no Internet connectivity, the brain would show that too. The first version of the brain is not to that level yet, but it&#8217;s in the right direction.

# Brain 1.0

Brain 1.0 is a Platsil GEL-10, silicone brain with a hollowed center. In the center is a plastic project box housing an Arduino with Neo Pixels attached to it. A Neo Pixel is an Adafruit project that is meant to be a low cost, multi-color LED that you can daisy-chain, or string together in-line. There&#8217;s really no reason to use Neo Pixels for this project besides the fact that I had some already.

Parts:

  * Halloween Brain Jello mold from Amazon
  * Platsil GEL-10 from [BITY Mold Supply][3]
  * Tupperware container donated to the cause
  * XL Breaking Bad meth making gloves
  * Mixing containers

# Making the Brain:

This was the most interesting part to me. I picked up the type of PlatSil that is a 1 to 1 compound either by volume or by mass so I didn&#8217;t need to worry about mixing too much. I took 500ML of A and mixed it with 500ML of B. This stuff has a 6 minute lifetime from the time you start mixing to the time it starts to harden. There are ways to slow this down, but again, I didn&#8217;t need to do that. I spent 2 minutes mixing because some guy on YouTube said this is important, and my recent adventures in Thermite taught me the lesson that they&#8217;re serious.

Before I poured it in, I used a can of [Pol-Ease 2300][4] release which is used to keep the brain separated from the Jello mold. I was reminded the hard way what happens when you forget this. Pouring it into the mold was pretty simple but I made a small clay holder for it so I could make sure it stayed level. After the contents were dumped in, I sunk the plastic project container that was going to be my hollowed inside.

The whole things hardens within 30 minutes but because mine was in the garage in October, it was more like an hour.

Because this stuff isn&#8217;t very cheap, I did a demo mold just to make sure I was on the right track.

[<img class="aligncenter size-thumbnail wp-image-679" alt="" src="http://www.antitree.com/wp-content/uploads/2013/10/IMG_20131024_1607041-150x150.jpg" width="150" height="150" />][5]

# PlatSil Gel-10:

My goal was to create a mold of a brain that was rubbery and brain like. This Platsil line of chemicals are designed to create molds for other things. There wasn&#8217;t a lot of people making actual things from the material itself but I really like the texture and toughness of using it as the model. I will say that it is 100% overkill for what I wanted. There&#8217;s probably someone that can recommend a better, cheaper**,** alternative but for me this worked in the time frame I needed it to. They have a bunch of different types and I really wanted light to diffuse through it so I got that translucent version. It still comes out pretty white depending on how thick of a mold you&#8217;re making.

<p style="text-align: center;">
  <a href="http://www.antitree.com/wp-content/uploads/2013/10/IMG_20131025_1707301.jpg"><img class="aligncenter  wp-image-680" alt="PlatSil Gel-10 A" src="http://www.antitree.com/wp-content/uploads/2013/10/IMG_20131025_1707301.jpg" width="360" height="480" /></a>
</p>

# Neo Pixel:

Neo Pixels are really slick. They have 4 leads on them. Power, Ground, signal in, and a signal out. The biggest benefit is that each pixel is individually addressable without the need for multiple connections. Pixel 0 connects to pixel 1 that connects to pixel N through a single  wire connected to your microcontroller or whatever you&#8217;re using.

&nbsp;

Power takes +5v, and there is a warning about memory consumption especially with smaller Arduinos and extremely long chains of Neo Pixels (up to 500 at 30 FPS). My 4 didn&#8217;t mind.

Adafruit has a [Neo Pixel library][6] that you can use pretty easily, even if you just want to hack one of their demos.

[<img class="aligncenter size-full wp-image-677" alt="Adafruit Neo Pixel" src="http://www.antitree.com/wp-content/uploads/2013/10/1060quattro_MED1.jpg" width="400" height="308" />][7]

# Arduino:

This is my hacked code to make the brain throb between red and pink. Again, a Neo Pixel is overkill for doing this but it&#8217;s fun none-the-less and I&#8217;ll be upgrading it next iteration.

<pre class="height-set:true height:150 lang:c decode:true" title="NeoPixel Halloween">#include &lt;Adafruit_NeoPixel.h&gt;
//Hacked from the original Adafruit library demo

#define PIN 6   //my control pin

// Parameter 1 = number of pixels in strip
// Parameter 2 = pin number (most are valid)
// Parameter 3 = pixel type flags, add together as needed:
//   NEO_KHZ800  800 KHz bitstream (most NeoPixel products w/WS2812 LEDs)
//   NEO_KHZ400  400 KHz (classic 'v1' (not v2) FLORA pixels, WS2811 drivers)
//   NEO_GRB     Pixels are wired for GRB bitstream (most NeoPixel products)
//   NEO_RGB     Pixels are wired for RGB bitstream (v1 FLORA pixels, not v2)
Adafruit_NeoPixel strip = Adafruit_NeoPixel(60, PIN, NEO_GRB + NEO_KHZ800);

void setup() {
  strip.begin();
  strip.show(); // Initialize all pixels to 'off'
}

void loop() {
  //Start out with a pink brain looking color
  colorWipe(strip.Color(255, 48, 48), 1); // Hot Pink

  //Throb read and then fade out
  heartThrob(20);
}

// Fill the dots one after the other with a color
void colorWipe(uint32_t c, uint8_t wait) {
  for(uint16_t i=0; i&lt;strip.numPixels(); i++) {
      strip.setPixelColor(i, c);
      strip.show();
      delay(wait);
  }
}

void rainbow(uint8_t wait) {
  //secret rainbow mode
  uint16_t i, j;

  for(j=0; j&lt;256; j++) {
    for(i=0; i&lt;strip.numPixels(); i++) {
      strip.setPixelColor(i, Wheel((i+j) & 255));
    }
    strip.show();
    delay(wait);
  }
}

void heartThrob(uint8_t wait) {
  uint16_t i, j;

  //Adjust 60 and 90 to the starting and ending colors you want to fade between. 
  for(j=60; j&lt;90; j++) {
    for(i=0; i&lt;strip.numPixels(); i++) {
      strip.setPixelColor(i, Wheel((i+j) & 255));
    }
    strip.show();
    delay(wait);
  }
}

// Input a value 0 to 255 to get a color value.
// The colours are a transition r - g - b - back to r.
uint32_t Wheel(byte WheelPos) {
  if(WheelPos &lt; 85) {
   return strip.Color(WheelPos * 3, 255 - WheelPos * 3, 0);
  } else if(WheelPos &lt; 170) {
   WheelPos -= 85;
   return strip.Color(255 - WheelPos * 3, 0, WheelPos * 3);
  } else {
   WheelPos -= 170;
   return strip.Color(0, WheelPos * 3, 255 - WheelPos * 3);
  }
}</pre>

<https://gist.github.com/antitree/7188144>

 [1]: http://www.antitree.com/wp-content/uploads/2013/10/2013-10-27.jpg
 [2]: http://www.antitree.com/wp-content/uploads/2013/10/2013-10-27-1.jpg
 [3]: http://www.shop.brickintheyard.com/PlatSil-Gel-10-Pint-Kit-2-Lbs-Gel10U2.htm
 [4]: http://www.shop.brickintheyard.com/Pol-Ease-2300-12-Oz-Spray-Can-2300.htm
 [5]: http://www.antitree.com/wp-content/uploads/2013/10/IMG_20131024_1607041.jpg
 [6]: https://github.com/adafruit/Adafruit_NeoPixel
 [7]: http://www.antitree.com/wp-content/uploads/2013/10/1060quattro_MED1.jpg