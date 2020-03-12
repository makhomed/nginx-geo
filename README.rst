nginx-geo
=========

Convert GeoLite2 Country database to nginx format.

Installation
------------

- ``cd /opt``
- ``git clone https://github.com/makhomed/nginx-geo.git nginx-geo``

Also you need to install ``curl`` and ``python``.

Upgrade
-------

- ``cd /opt/nginx-geo``
- ``git pull``

Usage
-----

.. code-block:: none

    usage: nginx-geo [-h] [--src SRC] [--dest DEST] [--work-dir WORKDIR]
                     [--nginx-pid PIDFILE] [--download] [--convert]
                     [--exclude-continent] [--exclude-ipv6] [--reload-nginx]

    Convert MaxMind GeoLite2 Country database to nginx format

    optional arguments:
      -h, --help           show this help message and exit
      --src SRC            uri of GeoLite2 Country database in CSV format
      --dest DEST          destination file name (geoip_country_code.conf)
      --work-dir WORKDIR   path to working directory (/etc/nginx/geo)
      --nginx-pid PIDFILE  path to nginx pid file (/var/run/nginx.pid)
      --download           download GeoLite2-Country-CSV.zip
      --convert            convert GeoLite2-Country-CSV.zip to nginx format
      --exclude-continent  exclude continent code if country code is unknown
      --exclude-ipv6       exclude IPv6 networks
      --reload-nginx       reload nginx

``--src`` by default is ``http://geolite.maxmind.com/download/geoip/database/GeoLite2-Country-CSV.zip``

``--dest`` by default is ``geoip_country_code.conf``

``--work-dir`` by default is ``/etc/nginx/geo``

``--nginx-pid`` by default is ``/var/run/nginx.pid``

Automation via cron
-------------------

Configure cron job, for example, in file ``/etc/cron.d/nginx-geo``:

.. code-block:: none

    RANDOM_DELAY=360

    0 0 * * * root /opt/nginx-geo/nginx-geo --download --convert --reload-nginx

Or, if only IPv4 networks needed:

.. code-block:: none

    RANDOM_DELAY=360

    0 0 * * * root /opt/nginx-geo/nginx-geo --download --convert --exclude-ipv6 --reload-nginx

nginx configuration
-------------------

.. code-block:: none

    geo $geoip_country_code {
        default XX;
        include /etc/nginx/geo/geoip_country_code.conf;
    }

