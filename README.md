# GeekMagicBootloader

A small stepping-stone firmware for the **GeekMagic SmallTV Ultra** (the 2.4″
square screen in a little TV case, ESP8266 inside). You install it once, through
the screen's own factory update page, and it then installs the real firmware for
you.

It exists because the factory firmware on newer screens refuses a full-size
firmware file with **`ERROR[4]: Not Enough Space`**. This bootloader is small
enough to get through, and once it is running the full firmware goes on with
room to spare.

It is not tied to any one project: it will install any firmware built for this
screen. [Glucogaze](https://glucogaze.com) uses it for GlucogazeMagic.

## Download

**[GeekMagicBootloader-v0.0.7-esp8266.bin](https://github.com/Glucogaze/GeekMagicBootloader/releases/latest/download/GeekMagicBootloader-esp8266.bin)**
— 432,784 bytes

SHA-256: `3ed0ddf43561becbe1227daa0424626fc9d1dccb1957a76820c1868a7be7e89d`

The link above always points at the newest release. Older versions are on the
[Releases](https://github.com/Glucogaze/GeekMagicBootloader/releases) page.

## Installing it

You need a PC or phone on a **2.4 GHz** Wi-Fi network, and the firmware you
want to end up with (a `.bin` file) saved somewhere you can find it.

### 1. Put the bootloader on the screen

1. Get the screen onto your Wi-Fi using its factory setup page, if it is not
   already.
2. Open the screen's web page in a browser (the address shows on the screen),
   choose **Update**, and upload `GeekMagicBootloader-v0.0.7-esp8266.bin`.
3. The screen restarts and says **GeekMagicBootloader** with a two-step bar
   under it.

### 2. Step 1 of 2 — join your Wi-Fi

1. On a phone or PC, join the Wi-Fi network called **`GeekMagic-Bootloader`**
   (no password).
2. Open **http://192.168.4.1**.
3. Pick your home network from the list, type its password, press
   **Save & connect**.
4. The page and the screen both show the address the screen now has on your
   network, for example `http://192.168.1.42`.

Got the password wrong, or picked a 5 GHz network? Nothing is lost: the
`GeekMagic-Bootloader` network stays up, so go back to 1 and try again.

### 3. Step 2 of 2 — upload the firmware

1. Rejoin your normal Wi-Fi.
2. Open the address from the previous step in a browser.
3. Choose the firmware `.bin` and upload it. The screen shows a percentage as
   it arrives.
4. When the screen says **INSTALLING / KEEP POWER ON**, leave it alone until
   the new firmware's own screen appears (about 30 seconds).

> **Do not unplug the screen while it says INSTALLING.** Pulling the power in
> that window can leave the screen unable to start. Any other time is safe: a
> power cut during the upload itself just brings the bootloader back.

That's it. The firmware you installed takes over, and the bootloader is gone.
Later updates come through the installed firmware; you only do this once per
screen.

From a PC, the upload in step 3 can also be done with `curl`:

```
curl -F "update=@GlucogazeMagic.bin" http://192.168.1.42/update
```

## What it will and won't accept

The bootloader checks every upload before writing anything. It refuses, and
tells you why on the screen, if the file is empty, is not an ESP8266 firmware
(ESP32 files are refused too), is under 200 KB, or arrives incomplete. A refused
upload changes nothing — the bootloader keeps running and you can try again.

`http://<screen address>/info` shows the bootloader version and the screen's
flash details as plain text, if you ever need them.

## Wi-Fi handover

The network name and password you typed in step 2 are left in the chip's own
Wi-Fi settings, where the installed firmware can pick them up on its first boot
and skip asking you again. GlucogazeMagic does this. A firmware that does not
look for them simply asks you for Wi-Fi as it normally would.

Nothing is baked into the bootloader file itself: one file works on every
screen, and no password of yours is in it.

## Versions

| version | size | notes |
|---|---|---|
| 0.0.7 | 432,784 bytes | current |
