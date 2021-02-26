~~~
      -----------------------
      PulseAudio |     Jack | User space
      -----------------------
      ALSA                  | Kernel space
      -----------------------
      Hardware	
      -----------------------
~~~


- Advanced Linux Sound Architecture [ Alsa ]

~~~
$ alsamixer
~~~

~~~
$ aplay audio.wav
~~~

- PulseAudio [ Require Dbus ]

~~~
$ pactl info
~~~

~~~
$ pavucontrol
~~~

~~~
$ pactl list modules
~~~

~~~
$ pactl load-module module-null-sink sink_name=Console sink_properties=device.description=Console
~~~

~~~
$ pactl unload-module <ID or NAME>
~~~

~~~
$ pactl list sinks [ output ]
~~~

~~~
$ pactl list sources [ input ]
~~~

- Jack [ backend dummy ]

~~~
$ jackd -R -d dummy -r 44100 -C 0 -P 2
~~~

~~~
$ jack_lsp
~~~

~~~
$ jack_lsp --connections
~~~

~~~
$ jack_disconnect system:playback_1 ardour:Click/audio_out\ 1
$ jack_disconnect system:playback_2 ardour:Click/audio_out\ 2
~~~

~~~
$ qjackctl
~~~

- My Setting PulseAudio

~~~
# /etc/pulse/default.pa

# REF: https://www.freedesktop.org/wiki/Software/PulseAudio/Documentation/User/Modules/#module-null-sink      (null output)
# REF: https://www.freedesktop.org/wiki/Software/PulseAudio/Documentation/User/Modules/#module-jack-source    (jack  input)
# REF: https://www.freedesktop.org/wiki/Software/PulseAudio/Documentation/User/Modules/#module-jack-sink      (jack output)
# REF: https://www.freedesktop.org/wiki/Software/PulseAudio/Documentation/User/Modules/#module-loopback

## JACK WITH BACKEND DUMMY

### NULL  OUTPUT
load-module module-null-sink sink_name=Sample  sink_properties=device.description=Sample
load-module module-null-sink sink_name=Capture sink_properties=device.description=Capture

### JACK INPUT
load-module module-jack-source source_name=jack_pulseaudio client_name=PulseAudio

### LOOPBACK JACK -> PULSEAUDIO
load-module module-loopback source=jack_pulseaudio sink=Sample latency_msec=1

### LOOPBACK SAMPLE MONITOR TO OUTPUT DEFAULT
load-module module-loopback source=Sample.monitor latency_msec=1
~~~

- My Setting Jack

~~~
# $HOME/.jackdrc

/usr/bin/jackd -R -d dummy -r 44100 -C 0 -P 2
~~~

- Ardour Plugins

~~~
- https://calf-studio-gear.org/
- https://github.com/x42/meters.lv2
- https://github.com/x42/x42-plugins
~~~
