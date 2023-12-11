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
- How to disable subltitles from cmdline?<br>

On pi3 fresh bullseye (arm64) install:
  - \$ startx & (needs allowed_users=anybody <br>
needs_root_rights=yes <br> in /etc/X11/Xwrapper.config)<br>
  - \$ export DISPLAY=:0
  - cvlc -I telnet --telnet-password=secret --telnet-port=9999 --aout alsa --alsa-audio-device hw:0,0 --x11-display=:0 --fullscreen /path/to/video.mp4
  - don't rely on numeric card id eg hw:0,0 - get identifier for sound card from 'aplay -l', eg<br>
  card 0: **vc4hdmi** [vc4-hdmi], device 0: MAI PCM i2s-hifi-0 [MAI PCM i2s-hifi-0]
  - cvlc -I telnet --telnet-password=secret --telnet-port=9999 --aout alsa --alsa-audio-device sysdefault:CARD=vc4hdmi --fullscreen
  - cvlc -I telnet --telnet-password=secret --telnet-port=9999 --aout alsa --alsa-audio-device sysdefault:CARD=vc4hdmi --vout mmal_xsplitter --fullscreen
  - \$ cvlc -I telnet --telnet-password=secret --telnet-port=9999 --aout alsa --alsa-audio-device sysdefault:CARD=vc4hdmi --vout drm_vout  --fullscreen
    
1a. VLC remote using HTTP web interface.   <br>
   Pause, FF, FR, seek work but no subtitle toggle <br>
   Add "--extraintf http --http-password foo" to cmdline, to enable access via http://pi3:8080 (default port).  This works on Android too<br>
   - <b>X not needed!</b>  Forcing video and audio to local hardware (--vout=drm_vout --alsa-audio-device sysdefault:CARD=vc4hdmi) means we can unset DISPLAY (otherwise it gets tunneled back) and even omit --no-xlib :<br>
unset DISPLAY ; cvlc --quiet --no-plugins-scan --sub-track-id 0 --no-video-title-show --play-and-exit --aout alsa --alsa-audio-device sysdefault:CARD=vc4hdmi --vout=drm_vout --fullscreen -I cli --extraintf http --http-password foo

1b. VLC using telnet remote.  This works, if a bit kludgy<br>
- On laptop:
   - telnet pi3 9999
   -   \> strack -1 # no subs
   -   \> strack 2 # use subtitles
   -   \> pause # etc (seek [secs], ...)
  

2. KODI...
