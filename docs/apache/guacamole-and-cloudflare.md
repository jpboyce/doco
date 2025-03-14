---
title: Guacamole and Cloudflare
date: 2025-03-14
categories:
    - Guacamole
    - Cloudflare
tags:
    - Guacamole
    - Cloudflare
---    
# Guacamole and Cloudflare

Guacamole is a remote access solution.  It uses a HTML 5 web front end to allow access to systems using remote access protocols like RDP and VNC.  A common use case for it is providing access to systems behind a firewall.

In some situations, this access may be complicated by factors such as not being able to or being unwilling to open ports.  Cloudflare has a Tunnel service where access can be given to systems in this situation without the need for inbound ports.  The architecture of this arrangement is shown below:

## Guacamole Server - Install

* Update packages using `#!shell sudo apt update`
* Install the prerequisite packages
``` shell
sudo apt install build-essential libcairo2-dev libjpeg-turbo8-dev \
 libpng-dev libtool-bin libossp-uuid-dev libvncserver-dev \
 freerdp2-dev libssh2-1-dev libtelnet-dev libwebsockets-dev \
 libpulse-dev libvorbis-dev libwebp-dev libssl-dev \
 libpango1.0-dev libswscale-dev libavcodec-dev libavutil-dev \
 libavformat-dev
```





| Step | Script/screenshot |
|---|---|
| Update packages | `sudo apt update` |
| Install prerequisite packages | <pre lang=shell><code>sudo apt install build-essential libcairo2-dev libjpeg-turbo8-dev \<br> libpng-dev libtool-bin libossp-uuid-dev libvncserver-dev \<br> freerdp2-dev libssh2-1-dev libtelnet-dev libwebsockets-dev \<br> libpulse-dev libvorbis-dev libwebp-dev libssl-dev \<br> libpango1.0-dev libswscale-dev libavcodec-dev libavutil-dev \<br> libavformat-dev </code></pre> |
| Get the latest release URL from the releases page and use wget to download it to the server | `sudo wget https://downloads.apache.org/guacamole/1.5.5/source/guacamole-server-1.5.5.tar.gz` |
| Unzip the download | `sudo tar -xvf guacamole-server-1.5.5.tar.gz` |
| Change to the extracted folder | `cd guacamole-server-1.5.5` |
| Run the build and install commands | <pre lang=shell><code>sudo ./configure --with-init-dir=/etc/init.d --enable-allow-freerdp-snapshots<br>sudo make<br>sudo make install</code></pre> |

Run the command to update the installed library cache

sudo ldconfig

## Title

| Name    | Code                      |
| ------- | ------------------------- |
| `Hello` | [code-hello](#code-hello) |

## Ref

### Code

<details id="code-hello" open>
<summary >code-hello</summary>

```go
package main

import "fmt"

func main() {
    fmt.Println("Hello, World!")
}
```

</details>