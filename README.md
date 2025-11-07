<br>

### *tested on arch hyprland with HyDE dotfiles configs*

<br>

  # Preview
  ![Preview](/preview.png)

<br>
  
  ## how does it work?
  as spotify sends signal which song its playing but does not trigger `org.freedesktop.Notifications.Notify()` as for arch hyprland.
  this script listens for Spotify track changes through **MPRIS** and displays desktop notifications with album artwork.

  
  ### Track Monitoring
  
  The script uses `playerctl` to listen for track changes:
  
  ```bash
  playerctl --player=spotify metadata --follow
  ```
  
  This provides live song metadata in the format:
  ```
  title | artist | mpris:artUrl
  ```
  
  `mpris:artUrl` is an HTTPS link to Spotify's album art, which the script downloads automatically.
  
  ### Album Art Handling
  
  `spotify-notify` fetches album artwork every time the track changes:
  
  ```bash
  curl -s "$arturl" -o /tmp/spotify_art.jpg
  ```
  
  The image is stored temporarily and then used in the notification.
  
  ### Notification Style
  
  The script uses Pango markup, which Dunst fully supports.
  *NOTE: in the preview image the styling of my notification other than text and song image sent from script was made by HyDE setup* 
    
  The title is bold and slightly larger than the artist name:
  
  ```xml
  <big><b>Song Title</b></big>
  Artist Name
  ```
  
  This keeps the look clean without being oversized.

  ### Local Song imported into spotify
  
  we just scan every song in the `MUSIC_DIR` and compare the output of `playerctl metadata title` to the song name embedded in the scanned song, if we get it pull out the song image from that file.
  
  ### Sending the Notification
  
  The notification is displayed using:
  
  ```bash
  notify-send "$message" -i /tmp/spotify_art.jpg -u normal
  ```
  
  The `-i` flag attaches the album artwork, giving a polished and modern notification.
  

<br>

# SETUP
  ### requirements
  1. `ffmpeg`
  2. `playerctl`

  ## make the script `spotify-notfiy` run when the user logs in
  
  <br>
  
  ## 1. making a systemd daemon

  
  1. get the script `spotify-notify` to `~./local/bin/`. (this path is completly optional you ccan put it any anywhere you want just remember the path)
  2. make it executable `chmod +x ~/.local/bin/spotify-notify`
  3. create the file `~/.config/systemd/user/spotify-notify.service` and paste this inside it 
  ```
  [Unit]
  Description=Spotify Track Change Notifications with Album Art
  
  [Service]
  ExecStart=%h/.local/bin/spotify-notify
  Restart=on-failure
  
  [Install]
  WantedBy=default.target
  ```
  4. reload all user services `systemctl --user daemon-reload`                                                                                                                             16:36 
  5. enable and start the daemon immediately `systemctl --user enable --now spotify-notify.service`

  and that's it you got the notification of spotify track change running.

  <br>
  
  ## if you any other method for making script run on startup, feel free to use your own way.

  ## NOTE: make sure dunstrc file is setup correct with `markup=full` also check your config for the dunst too if the script doesn't work.

  


  ## Want to contribute in making this script better? create a pull request!


  
