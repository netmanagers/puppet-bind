# Puppet module: bind

## [Maintainer wanted](https://github.com/netmanagers/puppet-bind/issues/new)

**WARNING WARNING WARNING**

[puppet-bind](https://github.com/netmanagers/puppet-bind) is not currently being maintained, 
and may have unresolved issues or not be up-to-date. 

I'm still using it on a daily basis (with Puppet 3.8.5) and fixing issues I find
while using it. But sadly, I don't have the time required to actively add new features,
fix issues other people report or port it to Puppet 4.x.

If you would like to maintain this module,
please create an issue at: https://github.com/netmanagers/puppet-bind/issues
offering yourself.

## Getting started

This is a Puppet module for bind based on the second generation layout ("NextGen") of Example42 Puppet Modules.

Updated by Javier Bertoli / Netmanagers

Made by Romain THERRAT / Carpe-Hora

Based on Example42 rules

Made by Alessandro Franceschi / Lab42

Official site: http://www.netmanagers.com.ar

Official git repository: http://github.com/netmanagers/puppet-bind

Released under the terms of Apache 2 License.

This module requires functions provided by the Example42 Puppi module (you need it even if you don't use and install Puppi)

For detailed info about the logic and usage patterns of Example42 modules check the DOCS directory on Example42 main modules set.

## USAGE - Basic management

* Install bind with default settings

        class { 'bind': }

* Install a specific version of bind package

        class { 'bind':
          version => '1.0.1',
        }

* Disable bind service.

        class { 'bind':
          disable => true
        }

* Remove bind package

        class { 'bind':
          absent => true
        }

* Enable auditing without without making changes on existing bind configuration files

        class { 'bind':
          audit_only => true
        }

## USAGE - Create a zone

* Create a zone as master.

        bind::zone { 'example.com':
          zone_serial => 1,
        }

* Create a zone as slave.

        bind::zone { 'example.com':
          zone_type => 'slave',
        }

* Explicitly set zone name.

        bind::zone { 'my-zone':
          zone_name => 'example.com',
          ...
        }

* Set TTL for zone.

        ...
        zone_ttl => '3600000',
        ...

* Set contact email address for zone. Default is root.${zone_name}. Don't forget final point.

        ...
        zone_contact => 'root.localhost.',
        ...

* Set time period for zone.

        ...
        zone_refresh   => '604800',
        zone_retry     => '86400',
        zone_expire    => '2419200',
        zone_neg_cache => '604800',
        ...

* Set name server for zone. Default is ns.${zone_name}.

        ...
        zone_ns => 'ns00',
        ...

## USAGE - Record

* Add an A record into zone (example.com). www.example.com -> 192.168.0.10. This will add entry like "www IN A 192.168.0.10".

        bind::record { 'www':
           zone   => 'example.com.',
           target => '192.168.0.10',
        }

* There is some wrapper for A, wildcard(*), AAAA, MX, TXT, SRV, CNAME and PTR entry. See source for more informations.

        bind::a
        bind::wildcard
        bind::aaaa
        bind::mx
        bind::txt
        bind::srv
        bind::cname
        bind::ptr

## USAGE - Overrides and Customizations
* Use custom sources for main config file

        class { 'bind':
          source => [ "puppet:///modules/lab42/bind/bind.conf-${hostname}" , "puppet:///modules/lab42/bind/bind.conf" ],
        }


* Use custom source directory for the whole configuration dir

        class { 'bind':
          source_dir       => 'puppet:///modules/lab42/bind/conf/',
          source_dir_purge => false, # Set to true to purge any existing file not present in $source_dir
        }

* Use custom template for main config file. Note that template and source arguments are alternative.

        class { 'bind':
          template => 'example42/bind/bind.conf.erb',
        }

* Automatically include a custom subclass

        class { 'bind':
          my_class => 'bind::example42',
        }


## USAGE - Example42 extensions management
* Activate puppi (recommended, but disabled by default)

        class { 'bind':
          puppi    => true,
        }

* Activate puppi and use a custom puppi_helper template (to be provided separately with a puppi::helper define ) to customize the output of puppi commands

        class { 'bind':
          puppi        => true,
          puppi_helper => 'myhelper',
        }

* Activate automatic monitoring (recommended, but disabled by default). This option requires the usage of Example42 monitor and relevant monitor tools modules

        class { 'bind':
          monitor      => true,
          monitor_tool => [ 'nagios' , 'monit' , 'munin' ],
        }

* Activate automatic firewalling. This option requires the usage of Example42 firewall and relevant firewall tools modules

        class { 'bind':
          firewall      => true,
          firewall_tool => 'iptables',
          firewall_src  => '10.42.0.0/24',
          firewall_dst  => $ipaddress_eth0,
        }


## CONTINUOUS TESTING

Travis {<img src="https://travis-ci.org/netmanagers/puppet-bind.png?branch=master" alt="Build Status" />}[https://travis-ci.org/netmanagers/puppet-bind]
