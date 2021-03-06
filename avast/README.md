malice-avast
============

[![License](http://img.shields.io/:license-mit-blue.svg)](http://doge.mit-license.org) [![Docker Stars](https://img.shields.io/docker/stars/malice/avast.svg)](https://hub.docker.com/r/malice/avast/) [![Docker Pulls](https://img.shields.io/docker/pulls/malice/avast.svg)](https://hub.docker.com/r/malice/avast/)

This repository contains a **Dockerfile** of [avast](https://www.avast.com/en-us/linux-server-antivirus) for [Docker](https://www.docker.io/)'s [trusted build](https://index.docker.io/u/malice/avast/) published to the public [DockerHub](https://index.docker.io/).

### Dependencies

-	[debian:jessie (*125 MB*\)](https://index.docker.io/_/debian/)

### Installation

1.	Install [Docker](https://www.docker.io/).
2.	Download [trusted build](https://hub.docker.com/r/malice/avast/) from public [DockerHub](https://hub.docker.com): `docker pull malice/avast`

### Usage

```
docker run --rm malice/avast EICAR
```

#### Or link your own malware folder:

```bash
$ docker run --rm -v /path/to/malware:/malware:ro malice/avast FILE

Usage: avast [OPTIONS] COMMAND [arg...]

Malice Avast AntiVirus Plugin

Version: v0.1.0, BuildTime: 20160618

Author:
  blacktop - <https://github.com/blacktop>

Options:
  --verbose, -V         verbose output
  --table, -t           output as Markdown table
  --post, -p            POST results to Malice webhook [$MALICE_ENDPOINT]
  --proxy, -x           proxy settings for Malice webhook endpoint [$MALICE_PROXY]
  --rethinkdb value     rethinkdb address for Malice to store results [$MALICE_RETHINKDB]
  --help, -h            show help
  --version, -v         print the version

Commands:
  update        Update virus definitions
  help          Shows a list of commands or help for one command

Run 'avast COMMAND --help' for more information on a command.
```

This will output to stdout and POST to malice results API webhook endpoint.

### Sample Output JSON:

```json
{
  "avast": {
    "infected": true,
    "result": "EICAR Test-NOT virus!!!",
    "engine": "2.1.2",
    "database": "16061703",
    "updated": "20160618"
  }
}
```

### Sample Output STDOUT (Markdown Table):

---

#### Avast
| Infected | Result                  | Engine | Updated  |
| -------- | ----------------------- | ------ | -------- |
| true     | EICAR Test-NOT virus!!! | 2.1.2  | 20160618 |

---

### To write results to [ElasticSearch](https://www.elastic.co/products/elasticsearch)

```bash
$ docker volume create --name malice
$ docker run -d -p 9200:9200 -v malice:/data --name elastic elasticsearch
$ docker run --rm -v /path/to/malware:/malware:ro --link elastic malice/avast -t FILE
```

### Documentation

To update the AV run the following:

```bash
$ docker run --name=avast malice/avast update
```

Then to use the updated avast container:

```bash
$ docker commit avast malice/avast:updated
$ docker rm avast # clean up updated container
$ docker run --rm malice/avast:updated EICAR
```

To add your own license and update

 - Download one from [here](https://www.avast.com/en-us/registration-free-antivirus.php)

```bash
$ docker run --name=avast -v /path/to/license:/etc/avast/license.avastlic malice/avast update
```

Then to use the license/updated avast container:

```bash
$ docker commit avast malice/avast:updated
$ docker rm avast # clean up updated container
$ docker run --rm malice/avast:updated EICAR
```

### Issues

Find a bug? Want more features? Find something missing in the documentation? Let me know! Please don't hesitate to [file an issue](https://github.com/maliceio/malice-av/issues/new) and I'll get right on it.

### TODO
 - [x] Add Docs for how to add your own Avast License
 - [ ] Add URL scanning

### License

MIT Copyright (c) 2016 **blacktop**
