---
created: 2024-12-15T17:49:25Z
modified: 2024-12-16T16:42:23Z
---
This month, I decided to rip my entire music collection…again -- this time directly to FLAC and subsequently transcoding to high-bitrate MP3. (Yeah, I know…two copies of every song in my library?! Have I completely lost it?! Actually, there's a method to my madness -- but that's a topic for another time.)

While working on my music library, I found myself using the following commands quite often. This was primarily so I could edit tags in a "staging area" and subsequently update copy them back to the location where they are served by [[Plex Media Server]].

## Backup music files

```shell
rsync -ahv --delete --list-only \
  /var/lib/plexmediaserver/Library/Media/Music/* \
  /mnt/data03/media/music
```

```shell
rsync -ahv --delete \
  /var/lib/plexmediaserver/Library/Media/Music \
  /mnt/data03/media/music
```

## Restore music files

```shell
rsync -ahv --delete --list-only \
  /mnt/data03/media/music \
  /var/lib/plexmediaserver/Library/Media/Music
```

```shell
rsync -ahv --delete \
  /mnt/data03/media/music \
  /var/lib/plexmediaserver/Library/Media/Music
```
