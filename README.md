# tmx
`tmx` is a short, simple, startup script for a glorious tmux HUD. Tmux is
initialized with this bash script that opens up a tmux with a special
.tmux.conf file depending on the type of connection:

    1. .tmux.conf: standard 
    2. .tmux.conf.nested: nested tmux session
    3. .tmux.conf.mini: small resolution screen version

Launch the script with 

    tmx $session_name
    # $session_name is any name for the default session
    # $session_name can be "-d" which is default and names the session after the HOST
    
It is recommended that you call this as soon as you open the shell, so
put this at the end your shell rc file (e.g. .zshrc, .bashrc or .bash_profile) file

     if [[ ! -n $TMUX ]] && [[ "$COMP_TYPE" != "central" ]] && [[ ! -n $SSH_CONNECTION ]] ; then
        # This checks if tmux exists, and if it does, runs the startup script tmx
        {  hash tmux 2>&- && tmx $(hostname -s) ; } || echo >&2 "tmux did not startup ..."
     fi

Other binaries are "statusline", "weather", "batterpower", and
"cpuUsage", which are setup to grab information for your tmux
status-line. 

* Weather grabs an rss feed for Tokyo, but just grab that link and go to
the source to find a city near you. Weather requires a variable to be in
the environment $LOGS_DIR, which it uses to save a data file for rapid
access and a symbols file for easy readability. Example symbols file is
in "example/weather.symbols". I recommended that you put LOGS_DIR in a
directory like Dropbox so that you keep your symbols and data linked
between computers. Even better, setup the following `cron` (by typing
`crontab -e`) to update your data file every couple hours. You don't
want to update too often or you will be hurting the bandwidth of the rss
feed, so be conservative. Use the following to update weather every two hours
    
        $ crontab -e 
        # in the editor, write this:
        0 */2 * * * /path_to_bin/weather --dump 

* Batterypower is setu to work on OS X, if anyone wants it to work
on linux, please let me know some configuration info to grab that
information and I'll update the script. 

* CpuUsage displays "cput% mem% and load#" and this works in both OS X
and linux machines that I have tested on.


