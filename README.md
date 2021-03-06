# puppet-prometheus
[![Puppet Forge](https://img.shields.io/puppetforge/e/brutus777/prometheus.svg)](https://forge.puppetlabs.com/brutus777/prometheus)
[![Puppet Forge](https://img.shields.io/puppetforge/v/brutus777/prometheus.svg)](https://forge.puppetlabs.com/brutus777/prometheus)
[![Puppet Forge](https://img.shields.io/puppetforge/f/brutus777/prometheus.svg)](https://forge.puppetlabs.com/brutus777/prometheus)

## Compatibility

| Prometheus Version  | Recommended Puppet Module Version   |
| ----------------    | ----------------------------------- |
| >= 0.16.2           | latest                              |

### What This Module Affects

* Installs the prometheus daemon, alertmanager or exporters(via url or package)
  * The package method was implemented, but currently there isn't any package for prometheus
* Optionally installs a user to run it under
* Installs a configuration file for prometheus daemon (/etc/prometheus/prometheus.yaml) or for alertmanager (/etc/prometheus/alert.rules)
* Manages the services via upstart, sysv, or systemd

## Usage

To set up a prometheus daemon:
On the server:

```puppet
class { '::prometheus':
  global_config  => { 'scrape_interval'=> '15s', 'evaluation_interval'=> '15s', 'external_labels'=> { 'monitor'=>'master'}},
  rule_files     => [ "/etc/prometheus/alert.rules" ],
  scrape_configs => [ { 'job_name'=> 'prometheus', 'scrape_interval'=> '10s', 'scrape_timeout'=> '10s', 'target_groups'=> [ { 'targets'=> [ 'localhost:9090' ], 'labels'=> { 'alias'=> 'Prometheus'} } ] } ]
}
```

or simply:
```puppet
include ::prometheus
```

On the monitored nodes:

```puppet
class { '::prometheus::node_exporter':
  collectors => ['diskstats','filesystem','loadavg','meminfo','netdev','stat,time']
}
```

or simply:
```puppet
include ::prometheus::node_exporter
```

For more information regarding class parameters please take a look at class docstring.

## Limitations/Known issues

Even if the module has templates for several linux distributions, only RH family distributions were tested.

## Development
Open an [issue](https://github.com/brutus333/puppet-prometheus/issues) or
[fork](https://github.com/brutus333/puppet-prometheus/fork) and open a
[Pull Request](https://github.com/brutus333/puppet-prometheus/pulls)
