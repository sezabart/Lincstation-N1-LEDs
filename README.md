# Lincstation N1 LEDs
Daemon to set the Lincstation N1 LEDs on Linux other than Unraid

The Lincstation N1 is a fine all-flash NAS. It uses mostly standard PC
hardware, which means you can run your own OS on top of it instead of
proprietary ones.

I installed CasaOS (actually Ubuntu Server (actually Debian Linux)) on mine. Unfortunately, if you do not use the
supplied freemium Unraid software, the LEDs will blink continuously, which is
particularly annoying in my case because they are in my peripheral vision.

Lincstation supplies a closed-source daemon with its Unraid distribution on a
USB stick (I removed mine), apparently written in Go but quite inefficient
because it makes the necessary I2C/SMBus calls to the LEDs by forking to the
i2c-tools utility `i2cset`. Github user ffalt reverse-engineered the protocol
in [this gist](https://gist.github.com/ffalt/984aa3644a90d4230eaf5b129aaf1eeb).

This fork makes it work for specifically the N1 on Ubuntu Server.


To build, simply run `make`. You will need to have the packages `i2c-tools`
and `i2c-tools-dev` (and optionally `i2c-tools-doc`) installed.

You can activate debug output on stdout by calling:

```
env LEDS_DEBUG=true lincstation_leds
```

By default, it will update the LEDs once per second. If you want something
more real-time, you can change `ACTIVITY_SAMPLE_INTERVAL` in the code to
something shorter. Just be aware that at 10 Hz, it consumes 2–3% CPU on mine.
