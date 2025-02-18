# Home Assistant Community Add-on: Pi-hole

[![GitHub Release][releases-shield]][releases]
![Project Stage][project-stage-shield]
[![License][license-shield]](LICENSE.md)

![Supports aarch64 Architecture][aarch64-shield]
![Supports amd64 Architecture][amd64-shield]
![Supports armhf Architecture][armhf-shield]
![Supports armv7 Architecture][armv7-shield]
![Supports i386 Architecture][i386-shield]

[![GitLab CI][gitlabci-shield]][gitlabci]
![Project Maintenance][maintenance-shield]
[![GitHub Activity][commits-shield]][commits]

Network-wide ad blocking

## About

[Pi-hole][pi-hole] is an advertising-aware DNS- and web server, meant to be run
on a dedicated Raspberry Pi connected to your home network. Pi-hole lets you
block advertisements for every device that connects to your network without the
need for any client-side software.

This add-on is a port of Pi-hole to be able to run on Home Assistant and is
based on Alpine Linux and is using Docker.

## Installation

The installation of this add-on is pretty straightforward and not different in
comparison to installing any other Home Assistant add-on.

1. **Ensure your Home Assistant device has a
   [static IP and static external DNS servers!](https://github.com/home-assistant/hassos/blob/dev/Documentation/network.md#static-ip)**
1. Search for the "Pi-hole" add-on in the Supervisor add-on store
   and install it.
1. Start the "Pi-hole" add-on
1. Check the logs of the "Pi-hole" add-on to see it in action.

## Configuration

**Note**: _Remember to restart the add-on when the configuration is changed._

Example add-on configuration:

```yaml
log_level: info
update_lists_on_start: true
ssl: false
certfile: fullchain.pem
keyfile: privkey.pem
interface: eth0
ipv6: true
ipv4_address: ''
ipv6_address: ''
hosts:
  - name: printer.local
    ip: 192.168.1.5
  - name: router.local
    ip: 192.168.1.1
  - name: router.local
    ip: "FE80:0000:0000:0000:0202:B3FF:FE1E:8329"
```

**Note**: _This is just an example, don't copy and paste it! Create your own!_

### Option: `log_level`

The `log_level` option controls the level of log output by the addon and can
be changed to be more or less verbose, which might be useful when you are
dealing with an unknown issue. Possible values are:

- `trace`: Show every detail, like all called internal functions.
- `debug`: Shows detailed debug information.
- `info`: Normal (usually) interesting events.
- `warning`: Exceptional occurrences that are not errors.
- `error`:  Runtime errors that do not require immediate action.
- `fatal`: Something went terribly wrong. Add-on becomes unusable.

Please note that each level automatically includes log messages from a
more severe level, e.g., `debug` also shows `info` messages. By default,
the `log_level` is set to `info`, which is the recommended setting unless
you are troubleshooting.

Using `trace` or `debug` log levels puts the dnsmasq daemon into debug mode,
allowing you to see all DNS requests in the add-on log.

### Option: `update_lists_on_start`

Download and process all configured ad block lists on add-on startup by setting
this option to `true`. This will add startup time to your add-on but will give
you the most recent versions of the ad block lists on start.

When this option is set to `false` you will still get updated lists once in a
while. A scheduled task will take care of that.

**Note**: _When starting the add-on for the very first time, the lists will be
updated, regardless of the value of this option._

### Option: `ssl`

Enables/Disables SSL (HTTPS) on the web interface of Pi-hole. Set it `true` to
enable it, `false` otherwise.

### Option: `certfile`

The certificate file to use for SSL.

**Note**: _The file MUST be stored in `/ssl/`, which is the default_

### Option: `keyfile`

The private key file to use for SSL.

**Note**: _The file MUST be stored in `/ssl/`, which is the default_

### Option: `interface`

Configures the interface the Pi-hole DNS server should be listening to. By
leaving it empty, the add-on will try to auto-detect the interface to use.

**Note**: _This option is in place in case auto-detection fails on your setup._

### Option: `ipv6`

Set this option to `false` to disable IPv6 support.

### Option: `ipv4_address`

Manually set the IPv4 address for Pi-hole to use. By leaving it empty, the
add-on will try to auto-detect the interface to use.

**Note**: _This option is in place in case auto-detection fails on your setup._

### Option: `ipv6_address`

Manually set the IPv6 address for Pi-hole to use. By leaving it empty, the
add-on will try to auto-detect the interface to use.

**Note**: _This option is in place in case auto-detection fails on your setup._

### Option: `hosts`

This option allows you create your own DNS entries for your LAN. This
capability can be handy for pointing easy to remember hostnames to an IP
(e.g., point `printer.local` to the IP address of your printer).

Add a list of hosts you want to add. Some hosts can have both IPv4 and IPv6
addresses. In that case, simply add the host twice (with both addresses).

See the example above this chapter for a more visual representation.

#### Sub-option: `name`

This option specifies the DNS name of the host you are adding. Its value could
be a short style hostname like: `printer` or a longer one `printer.local`.

#### Sub-option: `ip`

The IP address this specified host must point to. Its value must be an IPv6 or
IPv4 IP address.

### Option: `leave_front_door_open`

Adding this option to the add-on configuration allows you to disable
authentication on the admin interface by setting it to `true` and leaving the
password empty.

**Note**: _We STRONGLY suggest, not to use this, even if this add-on is
only exposed to your internal network. USE AT YOUR OWN RISK!_

## Using the Pi-hole integration in Home Assistant

Home Assistant offers a [Pi-hole integration][pi-hole-integration] that allows
you to retrieve statistics and interact with your Pi-hole installation.

To enable this integration, add the following lines to your `configuration.yaml`
file:

```yaml
# Example configuration.yaml entry
pi_hole:
  host: localhost:4865
  api_key: ""
```

For more information and documentation about configuring this sensor, please
check the [documentation of Home Assistant][pi-hole-integration].

## Changelog & Releases

This repository keeps a change log using [GitHub's releases][releases]
functionality. The format of the log is based on
[Keep a Changelog][keepchangelog].

Releases are based on [Semantic Versioning][semver], and use the format
of ``MAJOR.MINOR.PATCH``. In a nutshell, the version will be incremented
based on the following:

- ``MAJOR``: Incompatible or major changes.
- ``MINOR``: Backwards-compatible new features and enhancements.
- ``PATCH``: Backwards-compatible bugfixes and package updates.

## Trademark legal notice

This add-on is not created, developed, affiliated, supported, maintained
or endorsed by Pi-hole LLC.

All product names, logos, brands, trademarks and registered trademarks are
property of their respective owners. All company, product, and service names
used are for identification purposes only.

Use of these names, logos, trademarks, and brands does not imply endorsement.

## License

MIT License

Copyright (c) 2022 Martijn Becker
Copyright (c) 2017-2020 Franck Nijhof

Permission is hereby granted, free of charge, to any person obtaining a copy
of this software and associated documentation files (the "Software"), to deal
in the Software without restriction, including without limitation the rights
to use, copy, modify, merge, publish, distribute, sublicense, and/or sell
copies of the Software, and to permit persons to whom the Software is
furnished to do so, subject to the following conditions:

The above copyright notice and this permission notice shall be included in all
copies or substantial portions of the Software.

THE SOFTWARE IS PROVIDED "AS IS", WITHOUT WARRANTY OF ANY KIND, EXPRESS OR
IMPLIED, INCLUDING BUT NOT LIMITED TO THE WARRANTIES OF MERCHANTABILITY,
FITNESS FOR A PARTICULAR PURPOSE AND NONINFRINGEMENT. IN NO EVENT SHALL THE
AUTHORS OR COPYRIGHT HOLDERS BE LIABLE FOR ANY CLAIM, DAMAGES OR OTHER
LIABILITY, WHETHER IN AN ACTION OF CONTRACT, TORT OR OTHERWISE, ARISING FROM,
OUT OF OR IN CONNECTION WITH THE SOFTWARE OR THE USE OR OTHER DEALINGS IN THE
SOFTWARE.

[aarch64-shield]: https://img.shields.io/badge/aarch64-yes-green.svg
[amd64-shield]: https://img.shields.io/badge/amd64-yes-green.svg
[armhf-shield]: https://img.shields.io/badge/armhf-yes-green.svg
[armv7-shield]: https://img.shields.io/badge/armv7-yes-green.svg
[i386-shield]: https://img.shields.io/badge/i386-yes-green.svg
[issue]: https://github.com/kaliumxyz/addon-pi-hole/issues
[keepchangelog]: http://keepachangelog.com/en/1.0.0/
[license-shield]: https://img.shields.io/github/license/kaliumxyz/addon-pi-hole.svg
[maintenance-shield]: https://img.shields.io/maintenance/yes/2020.svg
[pi-hole-integration]: https://www.home-assistant.io/components/pi_hole/
[pi-hole]: https://pi-hole.net/
[releases-shield]: https://img.shields.io/github/release/kaliumxyz/addon-pi-hole.svg
[releases]: https://github.com/kaliumxyz/addon-pi-hole/releases
[repository]: https://github.com/kaliumxyz/repository
[semver]: http://semver.org/spec/v2.0.0.html
