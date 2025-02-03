# DNPM:DIP - Deployment/Operation Instructions

> 🚧 **Work in Progress**


## Known Issues

None a.t.m


## :arrow_right: Migration Notes

For sites connected to the central "NGINX Broker":

Due to a change of the server certificate issuing process (certificates are now issued by [HARICA](https://www.harica.gr/) on behalf of DFN),
the NGINX broker's new server certificate has a _new_ issuer chain.
In the NGINX _forward_ proxy configuration for "remote host verification", the previously used `certs/dfn-ca-cert.pem` file (containing the GEANT issuer chain) must thus be accordingly replaced.

Please either update your deployment repo and restart your docker setup to migrate, or perform the corresponding changes to the NGINX config manually


## Pre-requisites

* Recommended VM properties:
    * OS: Linux
    * RAM: min. 8 GB
    * CPU cores: min. 4
    * Disk: min. 20 GB
* Docker / Docker Compose

### Certificates

This deployment comes with dummy SSL certificates to be runnable out-of-the-box for testing purposes, but for production you'll need:

* DFN Server Certificate issued for your DNPM:DIP node's FQDN
* In case your site is to be connected to the "NGINX Broker" as part of the cross-site communication setup: Client Certificate issued by DNPM CA (see [here](https://ibmi-ut.atlassian.net/wiki/spaces/DAM/pages/2590714/Zertifikat-Verwaltung#Zertifikat-Verwaltung-BeantragungeinesClient-Zertifikats) for instructions to request one (Confluence login required))



## System Overview

![System Overview](System_Overview.png)


## Quick start / Test setup

- Pull this repository
- Run `./init.sh`
- Run `docker compose up`

This starts the components with the system's NGINX proxy running on `localhost:80`and `localhost:443` with the provided dummy SSL certificate.


Default login credentials: `admin / start123`


## Detailed Set-up

### Template Files 

Templates are provided for all configuration files.

- `./certs/*.template.*`
- `./backend-config/*.template.*`
- `./nginx/sites-available/*.template.*`
- `.env.template`

The provided `init.sh` script creates non-template copies of these for you to adapt locally, e.g. `./backend-config/config.template.xml -> ./backend-config/config.xml`. 

> ⚠️ **WARNING**
> 
> Please avoid making local changes to the versioned files in this repo (e.g. `docker-compose.yml`, `nginx.conf` or all `*.template.*`) 
> to avoid conflicts upon subsequent pulls on the repository. 
> All necessary configurations should rather occur via the non-versioned, non-template copies of respective files.


The following sections describe the meaning and necessary adaptations to the respective configuration files for customization of the setup.


-------
### Docker Compose Environment

Basic configuration occurs via environment variables in file `.env`.

| Variable                  |   Mandatory   | Use/Meaning                                                                                                                                                                                                                                                |
|---------------------------|:-------------:|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| `BASE_URL`                |      ✔️       | The base URL to your local DNPM:DIP node. In a production set-up, the hostname of the proxy behind which the components run (see below).                                                                                                                   |
| `BACKEND_LOCAL_SITE`      |      ✔️       | Your Site, in format `{Site-ID}:{Site-name}`, e.g. `UKT:Tübingen` (see [here](https://ibmi-ut.atlassian.net/wiki/spaces/DAM/pages/2613900/DNPM+DIP+-+Broker-Verbindungen) for the overview list of Site IDs and names in DNPM (Confluence Login required)) |
| `HTTPS_PORT`              |       ❌     | HTTPS Port of the NGINX reverse proxy (see below)                                                                                                                                                                                                          |
| `HTTP_PORT`               |       ❌     | HTTP Port of the NGINX reverse proxy (see below)                                                                                                                                                                                                           |
| `PULL_POLICY`             |       ❌     | docker compose [pull policy](https://docs.docker.com/reference/compose-file/services/#pull_policy)                                                                                                                                                                                                                         |
| `AUTHUP_SECRET`           |       ❌     | Password of the Authup `admin` user                                                                                                                                                                                                                        |
| `MYSQL_ROOT_PASSWORD`     |       ❌     | Password of the MySQL DB used in Authup                                                                                                                                                                                                                    |
| `BACKEND_CONNECTOR_TYPE`  |       ❌     | Set to one of { `broker`, `peer2peer` } to specify the desired connector type (see below)                                                                                                                                                                  | 
| `BACKEND_RD_RANDOM_DATA`  |       ❌     | Set to a positive integer to activate in-memory generation of Rare Diseases (RD) random data (for test purposes)                                                                                                                                           |
| `BACKEND_MTB_RANDOM_DATA` |       ❌     | Set to a positive integer to activate in-memory generation of Mol. Tumor Board (MTB) random data (for test purposes)                                                                                                                                       |
| `BACKEND_HGNC_GENESET_URL`|       ❌     | The DNPM:DIP application needs the [Complete HGNC Gene Set (JSON)](https://genenames.org/download/). By default, the official URL will be used to attempt downloading it, but in case this URL is not accessible from your setup infrastructure, you can override the default value e.g. to provide the URL of a proxy. | 

-------
### Proxy Setup

As shown in the system overview diagram above, the backend and frontend components are operated behind a reverse proxy.
In a production setting, this would handle TLS termination (including mutual TLS for API endpoints not secured by a login mechanism).
Also, unless your site were to use the Samply infrastructure with plain HTTP connection between Samply Proxy and DNPM:DIP backend, a forward proxy
is required to handle TLS to the broker server, and in case of the NGINX broker also the client certificate for mutual TLS (see below about the Backend Connector).

The default set-up uses NGINX as (reverse) proxy server.

Template configuration files for the respective proxy servers are in `nginx/sites-available`. 
The `init.sh` script creates non-template local copies of these in `nginx/sites-enabled`:

| File                                        | Meaning                                                           | 
|---------------------------------------------|-------------------------------------------------------------------|
| `nginx/sites-enabled/reverse-proxy.conf`     | Reverse Proxy using plain HTTP                                    |
| `nginx/sites-enabled/tls-reverse-proxy.conf` | Reverse Proxy using HTTPS (using provided dummy certificate)      |
| `nginx/sites-enabled/forward-proxy.conf`     | Forward Proxy for outgoing requests to DNPM:DIP peers             |

> **NOTE for NGINX experts**
>
> Although the usual procedure is to keep configs in `sites-available` and activate them using symbolic links in `sites-enabled`, we couldn't yet get this to work with the bound local directory.
> Hence the copying of actual config files in `sites-enabled`.
> This might be eventually corrected.

Aside from this, `./certs` contains the provided default certificates:

| File                      | Meaning                                                           | 
|---------------------------|-------------------------------------------------------------------|
| `./certs/dnpm-server-cert.pem` | Server certificate                                           |
| `./certs/dnpm-server-key.pem`  | Server certificate's private key                             |
| `./certs/dnpm-client-cert.pem` | Client certificate for use in mutual TLS with external peers |
| `./certs/dnpm-client-key.pem`  | Client certificate's private key                             |

This is bound to directory `/etc/ssl/certs` of the `nginx` container, to which certificates file paths in the configurations are pointing.

From this default set-up, you can make local customizations by adapting the respecitve files or removing them from `site-enabled`, as shown in the following examples.


#### HTTPS only

In a production setup, you will probably want to have the reverse proxy using only HTTPS, so remove the plain HTTP proxy configuration `.../sites-enabled/reverse-proxy.conf`.


#### Real Server and Client certificates

In production, you must provide you real server certificate and key, and in case your site uses the "NGINX Broker" for cross-site communication, also the client certificate and key for mutual TLS.
Provide these by either _overwriting_ the respective files in `./certs`, or simply adding these to `./certs` and _adapting_ the file paths in `tls-reverse-proxy.conf` and `forward-proxy.conf`.

> **NOTE**: Please do not overwrite the following certificate files:
>
>| File                      | Meaning                                                           | 
>|---------------------------|-------------------------------------------------------------------|
>| `./certs/ca-chain.pem`     | Certificate chain of the central broker's server certificate (for remote host verification)                     |
>| `./certs/dnpm-ca-cert.pem` | Certificate of the DNPM CA from which the client certificates originate (for client verification in mutual TLS) |


#### :bangbang: Securing Backend APIs

Some of the backend's API endpoints meant to be accessed by "system agents" instead of users via their browser are not protected by a login-based mechanism, and MUST be secured by other means.

| API | URI pattern | Purpose |
|--------|---------------|----------|
| Peer-to-Peer API | `/api/{usecase}/peer2peer/...` | Calls among DNPM:DIP nodes (status check, federated queries) | 
| [ETL API](https://github.com/KohlbacherLab/dnpm-dip-api-gateway/tree/8839e7dc3672144493c4fe22d94bc09154077126/app/controllers#example-data-and-etl-api) | `/api/{usecase}/etl/...` | Integration with local ETL setup (data upload, deletion) |

The following sections describe available options for the respective sub-API.


##### Peer-to-Peer API
---------

The setup varies depending on whether your site is connected to the "NGINX Broker" with inbound HTTPS or outbound-only HTTPS with the MIRACUM Polling Module, or whether you are connected to the Samply Broker.

**Case: NGINX Broker with inbound HTTPS**

The default security mechanism here is mutual TLS, as is already pre-configured.

In a production setting, however, you might use _different_ reverse proxy servers (with differents FQDNs) to expose the backend to internal clients (browser, ETL setup) separately from this "peer to peer API" destined for external calls (DNPM broker).
In this case, you could split the reverse proxy configuration accordingly, and perform the following adaptations:

- Remove the optional mutual TLS verification and `location ~ /api(/.*)?/peer2peer { ... }` from the _internal_ reverse proxy
- Make mutual TLS verification _mandatory_ in the external reverse proxy and only allow requests to the peer-to-peer API:
```nginx

  ...
  ssl_verify_client        on;
  ssl_verify_depth         1;

  location ~ /api(/.*)?/peer2peer {
    proxy_pass http://backend:9000;
  }

  # No other location entries!
```

**Case: NGINX Broker with Polling Module (outbound-only HTTPS)**

In this case, given that the "peer-to-peer API" is not directly exposed to incoming requests from the broker, but reached indirectly via the Polling Module
 (most likely running on the same VM), the mutual TLS check might be discarded altogether. 

**Case: Samply Beam Connect**

This case is similar to the above one: the "peer-to-peer API" is not directly exposed to incoming requests from the broker, but reached indirectly via Samply Beam Connect, so the mutual TLS check might be discarded altogether. 


##### ETL API
---------

Here are multiple possibilities to secure this sub-API.

**Mutual TLS**

As shown in the above system overview diagram, access to the ETL API could be secured by [mutual TLS](https://docs.nginx.com/nginx/admin-guide/security-controls/securing-http-traffic-upstream/).
For this, you would add a correponding check:
```nginx

  ...
  ssl_client_certificate   /etc/ssl/certs/my-ca-cert.pem;  # Path to trusted CA delivering the client cert used by the ETL stup
  ssl_verify_client        on;
  ssl_verify_depth         1;


  # Enforce mutual TLS for calls to ETL API endpoints
  location ~ /api(/.*)?/etl {
    if ($ssl_client_verify != "SUCCESS") {
       return 403;
    }
    proxy_pass http://backend:9000;
  }

```
However, whereas the validity of client certificates for mutual TLS on the peer-to-peer API (see above) is checked against the DNPM CA, you would use an internal CA here, or alternatively, white-list only the certificate used by the ETL setup:
```nginx
  ...
  ssl_client_certificate   /etc/ssl/certs/client-cert.pem;  # Client certificate(s) white-list 
  ssl_verify_client        on;
  ssl_verify_depth         0;
```

**HTTP Basic Authentication**

Alternatively, access to the ETL API can be secured using HTTP Basic Authentication, as described in [the NGINX docs](https://docs.nginx.com/nginx/admin-guide/security-controls/configuring-http-basic-authentication/). 


#### Forward Proxy

As mentioned above, TLS connections from the DNPM:DIP backend to the broker are handled by a forward proxy configured via `nginx/sites-enabled/forward-proxy.conf`. 

Adapt the hostname of the broker server in this file, especially in case your site uses the Samply Infrastructure, in order to point to your local Samply Beam Connect server.

> :warning: **NOTE**:
> In the template config file, the default activated `location` block uses _dynamic_ resolution of the defined upstream server.
> This is due to the fact that in the simpler case that's below, where a `proxy_pass` directive is used, the NGINX tries to check whether the configured upstream
> is reachable _upon start-up_ and when this fails, the NGINX service fails to start altogether. This issue is avoided with the default configuration using dynamic resolution,
> but at the disadvantage that this setup might also lead to errors of the form "no resolver defined to resolve ...", which requires configuring a `resolver` (see the template).
>
> Ideally, once the connection between the DNPM:DIP node setup and broker server (i.e. either thr "NGINX Broker" or Samply Beam Proxy) is possible, you should activate
> the second `location` block commented by default.


-------
### Backend

The following components/functions of the backend are configured via external configuration files.
These files are expected by the application in the directory `./backend-config` bound to docker volume `/dnpm_config` in `docker-compose.yml`.


-------
#### Play HTTP Server

The Play HTTP Server in which the backend application runs is configured via file `./backend-config/production.conf`.
The template provides defaults for all required settings.

##### Allowed Hosts

Given that the backend service is operated behind the reverse proxy and not exposed directly, the [Allowed hosts filter](https://www.playframework.com/documentation/3.0.x/AllowedHostsFilter) is disabled by default.
In case you were to deviate from this setup and directly expose the backend service, you should consider re-activateing this filter and configure the necessary allowed hosts accordingly:

```bash
...
hosts {
  allowed = ["your.host.name",...]
}
```
##### Payload size

Depending on the expected size of data uploads, the memory buffer can also be adjusted, e.g.:

```bash
...
http.parser.maxMemoryBuffer=10MB
```

-------
#### Persistence

Persistence by the backend uses docker volume `backend-data`. 

-------
#### Logging

Logging is based on [SLF4J](https://slf4j.org/).
The SLF4J implementation plugged in by the Play Framework is [Logback](https://logback.qos.ch/), which is configured via file `./backend-config/logback.xml`.
The default settings in the template define a daily rotating log file `dnpm-dip-backend[-{yyyy-MM-dd}].log` stored in sub-folder `/logs` of the docker volume bound for persistence (see above).

You might consider removing/deactivating the logging [appender to STDOUT](https://github.com/KohlbacherLab/dnpm-dip-deployment/blob/master/backend-config/logback.template.xml#L30)


-------
#### Application Config

The Backend application itself is configured via `./backend-config/config.xml`.
The main configuration item there depends on the type Connector used for communication with external DNPM:DIP node peers.

##### Case: `broker` Connector (Default)

The connector for the hub/spoke network topology used in DNPM, based on a central broker accessed via a local Broker Proxy (see system overview diagram).
The connector performs "peer discovery" by fetching the list of external peers from the central broker.

If desired, you can override the request time-out (seconds), and in case you prefer the connector to periodically update its "peer list", instead of just once upon start-up, set the period (minutes).


##### Case: `peer2peer` Connector

The connector based on a peer-to-peer network topology, i.e. with direct connections among DNPM:DIP nodes. (Provided only for potential backward compatibility with the "bwHealthCloud" infrastructure).

In this case, each external peer's Site ID, Name, and BaseURL must be configured in a dedicated element, as shown in the [template](https://github.com/KohlbacherLab/dnpm-dip-deployment/blob/master/backend-config/config.template.xml#L15).


## Further Links

* DNPM:DIP Backend: [REST API Docs](https://github.com/KohlbacherLab/dnpm-dip-api-gateway/blob/main/app/controllers/README.md)

