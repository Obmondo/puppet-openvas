# OpenVAS Puppet/OpenVox module

## Description

This module manages an OpenVAS (Greenbone Community Edition) deployment using
Docker Compose.

It creates and manages:

- OpenVAS compose directory and compose file
- Docker Compose stack lifecycle (`docker_compose { 'openvas': ... }`)
- Firewall rule for OpenVAS web interface exposure
- Docker engine and Docker Compose plugin (by default)

## Setup

### Setup requirements

- Puppet/OpenVox `>= 7.24 < 9.0.0`
- `puppetlabs/docker`
- `alexharvey/firewall_multi`
- `puppetlabs/firewall`
- `puppetlabs/stdlib`

### Beginning with openvas

```puppet
include openvas
```

## Usage

### Default usage

```puppet
include openvas
```

### Disable Docker management (if handled elsewhere)

```puppet
class { 'openvas':
  manage_docker => false,
}
```

### Install but keep web interface private

```puppet
class { 'openvas':
  install => true,
  expose  => false,
}
```

### Disable and remove managed resources

```puppet
class { 'openvas':
  install => false,
}
```

### Override compose settings

```puppet
class { 'openvas':
  compose_dir          => '/opt/openvas',
  feed_release         => '24.10',
  web_port             => 9392,
}
```

Notes:

- The compose file path is derived automatically as
  `${compose_dir}/docker-compose.yml`.
- `web_port` controls only the firewall rule port.
- The GSA container bind is fixed to `127.0.0.1:9392` in the managed compose
  template.

## Limitations

- Built and tested for Ubuntu (22.04, 24.04).
- By default (`manage_docker => true`) this module manages Docker and Docker
  Compose plugin.
- If you set `manage_docker => false`, Docker and Docker Compose plugin must
  already be present.

## Development

Run checks with PDK:

```bash
pdk validate
pdk test unit
```
