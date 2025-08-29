# nginx-geo (version 2.0.0)

Convert MaxMind GeoLite2 Country database to nginx format, for using via [`nginx geo module`](https://nginx.org/en/docs/http/ngx_http_geo_module.html)

Same as [`Cloudlare CF-IPCountry header`](https://developers.cloudflare.com/fundamentals/reference/http-headers/#cf-ipcountry) allow map visitor IPv4/IPv6 address 
to a two-character country code of the originating visitor's country.

Besides the [`ISO-3166-1 alpha-2 codes`](https://www.iso.org/iso-3166-country-codes.html), nginx-geo uses the following special country codes:

* XX - Used for clients without country code data.
* T1 - Used for clients using the Tor network.



## installation
> [!IMPORTANT]
> Python 3.9+ and [requests](https://requests.readthedocs.io/), [tomllib](https://docs.python.org/3/library/tomllib.html) or [tomli](https://pypi.org/project/tomli/) modules required.
```
cd /opt ; git clone https://github.com/makhomed/nginx-geo.git
```
Also create `nginx-geo.toml` configuration file, using provided `nginx-geo.toml.maxmind.example` or `nginx-geo.toml.ownhost.example` as examples of possible configurations.

## upgrade
```
cd /opt/nginx-geo ; git pull
```

## usage
```
/opt/nginx-geo/nginx-geo
```

## automation via cron

Configure cron job, for example, in file `/etc/cron.d/nginx-geo`:

```
RANDOM_DELAY=360

0 0 * * * root /opt/nginx-geo/nginx-geo
```

## nginx configuration in http context
```
    geo $remote_addr $geoip_country_code {
        include /etc/nginx/include/geoip_country_code.conf;
    }

    map $geoip_country_code $geoip_country_name {
        include /etc/nginx/include/geoip_country_name.conf;
    }

    proxy_set_header geoip_country_code $geoip_country_code;
    proxy_set_header geoip_country_name $geoip_country_name;
```

