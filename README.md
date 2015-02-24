# Puppet Storm
[![Puppet
Forge](http://img.shields.io/puppetforge/v/deric/storm.svg)](https://forge.puppetlabs.com/deric/storm) [![Build Status](https://travis-ci.org/deric/puppet-storm.svg?branch=master)](https://travis-ci.org/deric/puppet-storm)

Puppet module for managing Storm installation.

Features:

  * Hiera support
  * Mesos integration

## Requirements

  * binary package of Storm
    * [Debian/Ubuntu](https://github.com/deric/storm-deb-packaging)
  * Puppet >= 2.7

## Usage

The binary package, which is by default called `storm` will be installed on all machines. You can provide a set of packages:

```puppet
class {'storm':
  packages => ['storm', 'storm-mesos', 'libjzmq']
}
```

Main Storm class includes default configuration, each component is afterwards configured individually.

On master node include Nimbus:
```puppet
class {'storm::nimbus': }
```
By default service management is disabled, to enable starting service with OS default init system (init.d/upstart/systemd/...) use parameter `manage_service`:

```puppet
class {'storm::nimbus':
  manage_service => true
}
```

In order to change default service mechanism (determined by Puppet) use parameter `force_provider`:

```puppet
class {'storm::nimbus':
  manage_service => true,
  force_provider => 'runit'
}
```

You can adjust all the parameters directly:

```puppet
class {'storm::nimbus':
  host => '192.168.1.1'
}
```
or via Hiera:

```yaml
storm::nimbus::host: '192.168.1.1'
```

### UI

```puppet
class {'storm::ui': }
```

### Supervisor

Each computing node should include supervisor which watches Storm's bolts and spouts.

```puppet
class {'storm::supervisor': }
```
### DRPC

```puppet
class {'storm::drpc': }
```

### Mesos

Mesos integration requires special binary package which provides `storm-mesos` service (framework).

```puppet
class {'storm::mesos': }
```

In Hiera you could define something like this:

```yaml
storm::mesos::master_url: "%{mesos::zookeeper}"
```

### Packages

Some configuration is shared between components, e.g. packages that are installed:

```yaml
storm::packages: ['storm', 'libjzmq']
```
by default just `storm` package is installed.

## Installation

puppet-librarian:

```ruby
mod 'deric/storm', :git => 'https://github.com/deric/puppet-storm.git'
```

dependencies:
  * `puppetlabs/stdlib`
  * `puppetlabs/concat`
