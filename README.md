Simple cross-platform nginx configuration module.

Depends on -easel nginx builds:
  https://github.com/easel/nginx-centos-rpm
  https://launchpad.net/~erik-labianca/+archive/erik-labianca-ppa

Example usage:

node 'nginx-node' {
    # activate nginx and the base config
    include nginx;

    # create a basic proxy 
    nginx::proxy { "proxy_hostname":
        server_name => "backend_server_hostname"
        server_path => "/",
        server_port => "80",
        proxy_url   => "http://proxy.url"
    }

    # pull in some local configurations from a module
    file { "/etc/nginx/conf.d/proxy.conf":
	ensure => present,
	source => "puppet:///modules/firewall/nginx-proxy.conf",
	mode => 0644,
	owner => root,
	notify => Service[nginx]
    }
}

