# nginx-geo (version 2.0.0)

This tool converts the [MaxMind GeoLite2 Country database](https://dev.maxmind.com/geoip/geolite2-free-geolocation-data/) into files suitable for use with the [nginx geo module](https://nginx.org/en/docs/http/ngx_http_geo_module.html).

It allows to map the visitor’s IPv4 or IPv6 address to a two-character country code of the originating visitor's country. Besides the ISO-3166-1 alpha-2 codes, this tool uses the following [special country codes](https://developers.cloudflare.com/fundamentals/reference/http-headers/#cf-ipcountry):

* XX — Used when no country code is available for the client’s IP address.
* T1 — Used as a virtual country code for clients on the [Tor network](https://www.torproject.org/).

## Installation
> [!IMPORTANT]
> Python 3.9+ and the [requests](https://requests.readthedocs.io/) module are required.

- Clone this repository to the `/opt/nginx-geo` directory:
```bash
cd /opt && git clone https://github.com/makhomed/nginx-geo
```

- Create the `nginx-geo.toml` configuration file based on the provided examples:
```bash
cd /opt/nginx-geo && cp nginx-geo.toml.maxmind.example nginx-geo.toml && vim nginx-geo.toml
```

## Usage

- Create `/etc/cron.d/nginx-geo` with the following contents:
```cron
RANDOM_DELAY=360

0 0 * * * root /opt/nginx-geo/nginx-geo
```

- Add the following to `/etc/nginx/nginx.conf`:
```nginx
geo $remote_addr $geoip_country_code {
    include /etc/nginx/include/geoip_country_code.conf;
}

map $geoip_country_code $geoip_country_name {
    include /etc/nginx/include/geoip_country_name.conf;
}

proxy_set_header GeoIP-Country-Code $geoip_country_code;
proxy_set_header GeoIP-Country-Name $geoip_country_name;
```

