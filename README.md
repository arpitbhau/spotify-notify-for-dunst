## tested on arch hyprland with HyDE dotfiles configs


# SETUP
  ## make the script `spotify-notfiy` run when the user logs in

  ## 1. making a systemd daemon
  1. get the script `spotify-notify` to `~./local/bin/`. (this path is completly optional you ccan put it any anywhere you want just remember the path)
  2. create the file `~/.config/systemd/user/spotify-notify.service` and paste this inside it 
  ```
  [Unit]
  Description=Spotify Track Change Notifications with Album Art
  
  [Service]
  ExecStart=%h/.local/bin/spotify-notify
  Restart=on-failure
  
  [Install]
  WantedBy=default.target
  ```
  3. reload all user services `systemctl --user daemon-reload`                                                                                                                             16:36 
  4. enable and start the daemon immediately `systemctl --user enable --now spotify-notify.service`

  and that's it you got the notification of spotify track change running.

  ## if you any other method for making script run on startup, feel free to use your own way.

  ## NOTE: make sure dunstrc file is setup correct with `markup=full` also check your config for the dunst too if the script doesn't work.




  ## Want to contribute in making this script better? create a pull request!


  
