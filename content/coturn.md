---
title: "Coturn"
date: 2022-03-29
short_desc: 'A STUN and TURN server that allows users to perform WebRTC calls while being behind NATs.'
icon: "webrtc.svg"
img: "webrtc.svg"
tags: ['service']
---

[Coturn](https://github.com/coturn/coturn) is a libre **STUN** and
**TURN** server software that allows users of chat protocols (Such as
[XMPP](/prosody) and [Matrix](/matrix)) to perform WebRTC **voice
and video calls** despite them being behind NATs.

Almost every self-hosted voice and video conferencing program (such as
[Jitsi](/jitsi) and [Nextcloud\'s](/nextcloud) Talk app) will
**require** Coturn or some other equivalent turnserver to function
properly.

## Installation

Coturn is available in the Debian repositories:

```sh
apt install coturn
```

## Configuration

### Base configuration

Coturn\'s configuration file is `/etc/turnserver.conf`. There are a few
aspects that need to be changed in order to get a fully-functioning
turnserver.

Here is an example of some sane defaults:

```md
server-name={{<hl>}}turn.example.org{{</hl>}}
realm={{<hl>}}turn.example.org{{</hl>}}
listening-ip=your_public_ip

listening-port=3478
min-port=10000
max-port=20000

## The "verbose" option is useful for debugging issues
verbose
```

### Authentication

There are two options for authentication on a turnserver:

1.  **Usernames** and **passwords,**
2.  or **authentication secrets.**

Depending on what self-hosted service is being used in conjunction with
Coturn, you may need one or the other of these two options.

#### Usernames and Passwords

To utilize username and password authentication with Coturn, add the
following configuration in `turnserver.conf`:

```txt
lt-cred-mech
user=username:password
```

#### Authentication Secrets

To utilize authentication secrets with Coturn, add the following
configuration in `turnserver.conf`:

```txt
use-auth-secret
static-auth-secret={{<hl>}}your_auth_secret{{</hl>}}
```

## TURNS (TLS Encryption)

Some self-hosted services (such as Matrix and XMPP) may support the use
of **TURNS:** An encrypted version of TURN, which allows for WebRTC
connections to be established with the use of an encrypted TLS tunnel,
just like HTTPS allows for encrypted viewing of websites.

To utilize TURNS, certificates need to be declared for
**turn.example.org** in `turnserver.conf`:

```txt
cert=/etc/letsencrypt/live/{{<hl>}}turn.example.org{{</hl>}}/fullchain.pem
pkey=/etc/letsencrypt/live/{{<hl>}}turn.example.org{{</hl>}}/privkey.pem
```

## Starting Coturn

After all configuration changes are complete, Coturn can be started with
its systemd daemon:

```sh
systemctl restart coturn
```

Congratulations! You\'ve successfully setup a Coturn server!

------------------------------------------------------------------------

*Written by [Denshi.](https://denshi.org)

Donate Monero at 48dnPpGgo8WernVJp5VhvhaX3u9e46NujdYA44u8zuMdETNC5jXiA9S7JoYMM6qRt1ZcKpt1J3RZ3JPuMyXetmbHH7Mnc9C
[\[QR\]](https://denshi.org/images/xmr.png)*
