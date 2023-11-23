# RasPi-fun
Issues, thoughts and updates related to my Raspberry Pis

### My Pis
- pi1 just used for mpd, streaming 6music most of the time
- pi2 ethernet-attached to router.  Runs the whats-playing bots, also connects to VPN and has pihole running for that.
- pi3 Main box - mpd running for music, omxplayer to play video to TV via HDMI

### 23 Nov 2023 OMXplayer vs VLC
Upgraded pi1, pi2 and pi3 crom bullseye to bookworm.  All good except realised omxplayer is deprecated and I should be using an alternative (VLC).
- VLC, while it has a command-line version (cvlc), needs to be told to use HDMI for video and also for audio.  No keyboard controls as far as I'm aware so no fast-forward, pause, subtitle-toggle...
-  unset DISPLAY ; cvlc --no-xlib --aout alsa --alsa-audio-device hw:0,0 /path/to/video.mp4

