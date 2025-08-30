# nginx-geo (version 2.0.0)

This tool converts the [MaxMind GeoLite2 Country database](https://dev.maxmind.com/geoip/geolite2-free-geolocation-data/) into files suitable for use with the [nginx geo module](https://nginx.org/en/docs/http/ngx_http_geo_module.html).

This tool maps a visitor’s IPv4 or IPv6 address to a two-letter country code. In addition to the ISO 3166-1 alpha-2 codes, this tool — like Cloudflare’s [CF-IPCountry header](https://developers.cloudflare.com/fundamentals/reference/http-headers/#cf-ipcountry) — uses the following special country codes:

* XX — Used when no country code is available for the client’s IP address.
* T1 — Used as a virtual country code for clients on the [Tor network](https://www.torproject.org/).

## Installation

> [!IMPORTANT]
> Python 3.9+ and the [requests](https://requests.readthedocs.io/) module are required.

- Clone this repository into the `/opt/nginx-geo` directory:

```bash
cd /opt && git clone https://github.com/makhomed/nginx-geo.git nginx-geo
```

- Create `nginx-geo.toml` configuration file based on the provided examples:

```bash
cd /opt/nginx-geo && cp nginx-geo.toml.maxmind.example nginx-geo.toml && vim nginx-geo.toml
```

## Upgrade

```bash
cd /opt/nginx-geo && git pull
```

## Usage

- Create the `/etc/cron.d/nginx-geo` file with the following contents:

```cron
RANDOM_DELAY=360

0 0 * * * root /opt/nginx-geo/nginx-geo
```

- Add the following to the `/etc/nginx/nginx.conf` file:

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

