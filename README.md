# mondedie/rutorrent

[![Build Status](https://travis-ci.com/mondediefr/docker-rutorrent.svg?branch=master)](https://travis-ci.com/mondediefr/docker-rutorrent)
[![](https://img.shields.io/docker/pulls/mondedie/rutorrent)](https://hub.docker.com/r/mondedie/rutorrent)
[![](https://img.shields.io/docker/stars/mondedie/rutorrent)](https://hub.docker.com/r/mondedie/rutorrent)

## Features

 - Based on Alpine Linux.
 - rTorrent and libtorrent are compiled from source.
 - Provides by default a solid configuration.
 - No **ROOT** process.
 - Persitance custom configuration for rutorrent and rtorrent.
 - Add your own plugins and themes.
 - Filebot is included, and creates symlinks in `/data/media` (choose filebot tag).

## Tag available

 - latest [(Dockerfile)](https://github.com/mondediefr/docker-rutorrent/blob/master/Dockerfile)
 - filebot [(Dockerfile)](https://github.com/mondediefr/docker-rutorrent/blob/master/Dockerfile)

## BUILD IMAGE

### Build arguments

| Argument | Description | Type | Default value |
| -------- | ----------- | ---- | ------------- |
| **RTORRENT_VER** | rtorrent version | *optional* | v0.9.8
| **LIBTORRENT_VER** | libtorrent version | *optional* | v0.13.8
| **LIBZEN_VER** | libzen version | *optional* | 0.4.37
| **LIBMEDIAINFO_VER** | libmediainfo version | *optional* | 19.09
| **GEOIP_VER** | pecl geoip version | *optional* | 1.1.1
| **FILEBOT** | Build with filebot | *optional* | NO
| **FILEBOT_VER** | Filebot version | *optional* | 4.8.5
| **CHROMAPRINT_VER** | Chromaprint version | *optional* | 1.4.3

### build

```sh
docker build -t mondedie/rutorrent:latest https://github.com/mondediefr/docker-rutorrent.git
```

### Build with arguments

```sh
docker build -t mondedie/rutorrent:filebot \
  --build-arg FILEBOT=YES \
  --build-arg RTORRENT_VER=v0.9.7 \
  --build-arg LIBTORRENT_VER=v0.13.7 \
  https://github.com/mondediefr/docker-rutorrent.git
```

## Configuration

### Environment variables

| Variable | Description | Type | Default value |
| -------- | ----------- | ---- | ------------- |
| **UID** | Choose uid for launch rtorrent, must be a number | *optional* | 991
| **GID** | Choose gid for launch rtorrent, must be a number | *optional* | 991
| **WEBROOT** | Webroot path for rutorrent | *optional* | /
| **PORT_RTORRENT** | Port of rtorrent | *optional* | 45000
| **DHT_RTORRENT** | DHT option in rtorrent.rc file | *optional* | off
| **CHECK_PERM_DATA** | Check permission data in downloads directory | *optional* | true

### Environment variables with filebot

| Variable | Description | Type | Default value |
| -------- | ----------- | ---- | ------------- |
| **FILEBOT_LICENSE** | License file path | **required** | none
| **FILEBOT_RENAME_METHOD** | Method for rename media | *optional* | symlink
| **FILEBOT_RENAME_MOVIES** | Regex for rename movies | *optional* | "{n} ({y})"
| **FILEBOT_RENAME_MUSICS** | Regex for rename musics | *optional* | "{n}/{fn}"
| **FILEBOT_RENAME_SERIES** | Regex for rename series | *optional* | "{n}/Season {s.pad(2)}/{s00e00} - {t}"
| **FILEBOT_RENAME_ANIMES** | Regex for rename animes | *optional* | "{n}/{e.pad(3)} - {t}"
| **FILEBOT_LANG** | Set your language | *optional* | fr
| **FILEBOT_CONFLICT** | Conflict management | *optional* | skip

### Volumes

 - `/data` : folder for download torrents
 - `/config` : folder for rtorrent and rutorrent configuration

#### Data folder tree

 - `/data/.watch` : rtorrent watch directory
 - `/data/.session` : rtorrent save statement here
 - `/data/downloads` : rtorrent download torrent here
 - `/data/media` : organize your media and create a symlink with filebot
 - `/config/rtorrent` : path of .rtorrent.rc
 - `/config/rutorrent/conf` : global configuration of rutorrent
 - `/config/rutorrent/share` : rutorrent user configuration and cache
 - `/config/custom_plugins` : add your own plugins
 - `/config/custom_themes` : add your own themes
 - `/config/filebot` : add your License file in this folder

### Ports

 - 8080
 - PORT_RTORRENT (default : 45000)

## Usage

### Simple launch

```sh
docker run --name rutorrent -dt \
  -e UID=1000 \
  -e GID=1000 \
  -p 8080:8080 \
  -p 45000:45000 \
  -v /mnt/docker/rutorrent/config:/config \
  -v /mnt/docker/rutorrent/data:/data \
  mondedie/rutorrent:latest
```

URI access : http://xx.xx.xx.xx:8080

### Advanced launch

Add custom plugin :

```sh
mkdir -p /mnt/docker/rutorrent/config/custom_plugins
git clone https://github.com/Gyran/rutorrent-ratiocolor.git /mnt/docker/rutorrent/config/custom_plugins
```

Run container :

```sh
docker run --name rutorrent -dt \
  -e UID=1000 \
  -e GID=1000 \
  -e DHT_RTORRENT=on \
  -e PORT_RTORRENT=6881 \
  -e FILEBOT_LICENSE=/config/filebot/FileBot_License_XXXXXXXXX.psm \
  -e FILEBOT_RENAME_METHOD=move \
  -e FILEBOT_RENAME_SERIES="{n}/Season {s}/{n} - {s00e00} - {t}" \
  -p 9080:8080 \
  -p 6881:6881 \
  -p 6881:6881/udp \
  -v /mnt/docker/rutorrent/config:/config \
  -v /mnt/docker/rutorrent/data:/data \
  mondedie/rutorrent:filebot
```

URI access : http://xx.xx.xx.xx:9080/rutorrent

## License

Docker image [mondedie/rutorrent](https://hub.docker.com/r/mondedie/rutorrent) is released under [MIT License](https://github.com/mondediefr/docker-rutorrent/blob/master/LICENSE).
