# RasPi-fun
Issues, thoughts and updates related to my Raspberry Pis

### My Pis
- pi1 just used for mpd, streaming 6music most of the time
- pi2 ethernet-attached to router.  Runs the whats-playing bots, also connects to VPN and has pihole running for that.
- pi3 Main box - mpd running for music, omxplayer to play video to TV via HDMI

### 23 Nov 2023 OMXplayer vs other options
1. VLC - needs to be run (and controlled) from a remote machine since the Pi has no keyb/mouse connected.
Upgraded pi1, pi2 and pi3 from bullseye to bookworm.  All good except realised omxplayer is deprecated and I should be using an alternative (VLC).
- VLC, while it has a command-line version (cvlc), needs to be told to use HDMI for video and also for audio.  No keyboard controls as far as I'm aware so no 'seek forward/back', pause, subtitle-toggle...
- Downgraded system on pi3 to buster (using rsync from an backup)
- VLC is 3.0.17.4 (from raspbian repositories)
- Get basic VLC working...<br>
   cmd: unset DISPLAY ; cvlc --no-xlib --aout alsa --alsa-audio-device hw:0,0 /path/to/video.mp4
   - aout alsa otherwise pulseaudio kicks in
   - alsa-audio-device hw:0,0 using info from 'aplay -l')
   - video plays fine (a bit of delay at start, subtitles on by default)
- How to disable subltitles from cmdline?
- There is a web interface to control VLC.   Add "-I http --http-password foobar" to cmdline, access http://pi3:8080 <br>
   Pause, FF, FR, seek work but no subtitle toggle

1a. VLC using telnet remote.  This works, if a bit kludgy<br>
- On pi3:
  - startx &
  - cvlc -I telnet --telnet-password=secret --telnet-port=9999 --aout alsa --alsa-audio-device hw:0,0 --x11-display=:0 --fullscreen /path/to/video.mp4
- On laptop:
   - telnet pi3 9999
   -   \> strack -1 # no subs
   -   \> strack 2 # use subtitles
   -   \> pause # etc (seek [secs], ...)
  

2. KODI...
