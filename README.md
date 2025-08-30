# nginx-geo (version 2.0.0)

Convert the MaxMind GeoLite2 Country database to the nginx format, to use with the [`nginx geo module`](https://nginx.org/en/docs/http/ngx_http_geo_module.html).

Same as provided by Cloudflare [`CF-IPCountry header`](https://developers.cloudflare.com/fundamentals/reference/http-headers/#cf-ipcountry) allows mapping visitor IPv4 or IPv6 address\
to a two-character country code of the originating visitor's country.

In addition to the [`ISO-3166-1 alpha-2 codes`](https://www.iso.org/iso-3166-country-codes.html), nginx-geo uses the following special country codes:

* XX - Used for clients without country code data.
* T1 - Used for clients using the Tor network.

## Installation
> [!IMPORTANT]
> Python 3.9+ and the [requests](https://requests.readthedocs.io/), [tomllib](https://docs.python.org/3/library/tomllib.html) or [tomli](https://pypi.org/project/tomli/) modules are required.
```
cd /opt ; git clone https://github.com/makhomed/nginx-geo.git
```

Also create an `/opt/nginx-geo/nginx-geo.toml` configuration file using the provided\
`nginx-geo.toml.maxmind.example` or `nginx-geo.toml.ownhost.example` as configuration examples.

## Upgrade
```
cd /opt/nginx-geo ; git pull
```

## Usage
```
# /opt/nginx-geo/nginx-geo
```

## Automation
```
# cat /etc/cron.d/nginx-geo

RANDOM_DELAY=360

0 0 * * * root /opt/nginx-geo/nginx-geo
```

## Configuration
```
# cat /etc/nginx/nginx.conf

    geo $remote_addr $geoip_country_code {
        include /etc/nginx/include/geoip_country_code.conf;
    }

    map $geoip_country_code $geoip_country_name {
        include /etc/nginx/include/geoip_country_name.conf;
    }

    proxy_set_header geoip-country-code $geoip_country_code;
    proxy_set_header geoip-country-name $geoip_country_name;
```

