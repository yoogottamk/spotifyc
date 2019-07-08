# spotifyc

Mostly a wrapper around playerctl, with a few extra features 😉

## How to use
Clone this repository and add it in your `PATH`.
Alternatively, you can place both the executables in some directory which is in `PATH`

## Features [in no specific order]
 - Basic track control:
    ```sh
    $ spotifyc [-c|-p|-n|-r|-s]
    ```
        -c: toggle play/pause state
        -p: previous track
        -n: next track
        -r: play [why r?! resume]
        -s: pause [why s?! stop]


 - Control spotify's volume:
    ```sh
    $ spotifyc -v [m|u|t|abs|+-rel]
    ```
        m: mute
        u: unmute
        t: toggle mute state
        abs: set spotify's volume to abs
        +/-rel: adjust spotify's volume by rel


 - Get formatted output for status bars like [polybar](https://github.com/polybar/polybar),
    [i3blocks](https://github.com/vivien/i3blocks), etc
    (basically any status bar which allows you to run a script)
    ```sh
    $ spotifyc -f '<format>' -i '<playing>' '<paused>' -o
    ```
        -f '<format>': specify the format of output
                        [see formatting options below]
        -i '<playing>' '<paused>': specify the icons to be displayed when
                                    song is playing or is paused repectively
        -o: prints the output in the specified format [defaults to '{{ artist }}: {{ title }}'

    Example polybar config:

    ```ini
    [module/spotify]
    type = custom/script
    format-underline = #2aa198
    interval = 1

    exec = spotifyc -f "{{ icon  }} {{ artist  }}: {{ title  }}" -i "" "" -o
    exec-if = pgrep -x spotify

    label = %output:0:37:...%
    format = %{A1:spotifyc -p:}%{A} <label> %{A1:spotifyc -n:}%{A}

    click-left = spotifyc -c
    click-right = spotifyc -v t
    ; adjust this if you have enabled natural scrolling
    scroll-up = spotifyc -v '+5'
    scroll-down = spotifyc -v '-5'
    ```

    ![polybar example](./polybar_eg.png)

 - Mute ads on spotify and play some other song stored locally.

    ```sh
    $ spotifyc -m '<alt_music_dir>'
    ```
        -m: mutes ads on spotify
        if provided, a random song present in 'alt_music_dir' will be played, and then spotify will resume again

    ## NOTE
    Make sure only one instance is running at a time
    [you don't want 2 different songs playing whenever an ad comes]

    Make sure you have nothing other than songs in 'alt_music_dir'
    [since vlc can play so many formats, having a check in code didn\'t make sense]

    If you have songs shorter in length than the advertisement, you unfortunately
    will have to listen to the ad, and maybe resume spotify again.

    ### How to run?
    If you simply run this command, it will block your shell.
    Add an `&` at the end to run this in background.
    Ideally place this in some file while which runs when you log in [not `.bashrc` or `.zshrc`].
    In the standard case, `~/.profile` should work.

    ### Inspiration
    Spotify ads which complain about how ads on Spotify are bad and irritating

## Formatting output
This uses playerctl to display metadata. So, it supports all options which playerctl does. 
Check `man playerctl` to see all options and modifiers.

Other than that, it also uses '{{ icon }}' which is used to display the icons specified by `-i|--icons`

## Requirements
This requires [playerctl](https://github.com/acrisci/playerctl), pactl(pulseaudio), python3 and vlc bindings for python3

    pip3 install python-vlc

## Issues
Please create issues on [GitHub](https://github.com/YoogottamK/spotifyc/issues)

## Note
This project was created just for fun. If you really like Spotify and hate ads, consider paying for the premium account.
