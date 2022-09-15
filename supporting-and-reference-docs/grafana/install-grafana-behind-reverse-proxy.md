# Install Grafana Behind reverse proxy

If the installation is done on a Host server then the following steps should be performed to setup grafana behind a reverse proxy.

### Installation steps

1. To let Grafana know how to render and redirect the links correctly, in the Grafana configuration file, change `server.domain` to the domain name that will be used,

```bash
[server]
domain = example.com
```

2\. Restart grafana to see the changes

3\. To serve grafana behind the sub path, such as [`http://example.com/grafana`](http://example.com/grafana)``

Include the sub path at the end of the `root_url.`

Set `serve_from_sub_path` to `true.`

```
[server]
domain = example.com
root_url = %(protocol)s://%(domain)s:%(http_port)s/grafana/
serve_from_sub_path = true
```

4\. Next step is to configure NGINX.

### Configure NGINX

NGINX is a high performance load balancer, web server, and reverse proxy.

* In the NGINX configuration file inside `http` section, add the following:

```
// this is required for proxy Grafana Live WebSocket connections.
map $http_upgrade $connection_upgrade {
  default upgrade;
  '' close;
}

upstream grafana {
  server localhost:3000;
}

server {
  listen 80;
  root /usr/share/nginx/html;
  index index.html index.htm;

  location / {
    proxy_set_header Host $http_host;
    proxy_pass http://grafana;
  }

  # Proxy Grafana Live WebSocket connections.
  location /api/live/ {
    proxy_http_version 1.1;
    proxy_set_header Upgrade $http_upgrade;
    proxy_set_header Connection $connection_upgrade;
    proxy_set_header Host $http_host;
    proxy_pass http://grafana;
  }
}
```

* Reload the NGINX configuration
* Navigate to port 80 on the machine NGINX is running on and the Grafana login page will appear.

{% hint style="info" %}
For Grafana Live which uses WebSocket connections, raise a Nginx [worker\_connections](https://nginx.org/en/docs/ngx\_core\_module.html#worker\_connections) option which is 512 by default – which limits the number of possible concurrent connections with Grafana Live.
{% endhint %}

{% hint style="danger" %}
Also, be aware that the above configuration will work only when the **`proxy_pass`** value for **`location /`** is a literal string. If you are using a variable here, [read this GitHub issue](https://github.com/grafana/grafana/issues/18299) and there will be a need to add [an appropriate NGINX rewrite rule](https://www.nginx.com/blog/creating-nginx-rewrite-rules/).
{% endhint %}

To configure NGINX to serve Grafana under a _sub path_, update the `location` block:

```
// this is required to proxy Grafana Live WebSocket connections.
map $http_upgrade $connection_upgrade {
  default upgrade;
  '' close;
}

upstream grafana {
  server localhost:3000;
}

server {
  listen 80;
  root /usr/share/nginx/www;
  index index.html index.htm;

  location /grafana/ {
    rewrite  ^/grafana/(.*)  /$1 break;
    proxy_set_header Host $http_host; 
    proxy_pass http://grafana;
  }

  # Proxy Grafana Live WebSocket connections.
  location /grafana/api/live/ {
    rewrite  ^/grafana/(.*)  /$1 break;
    proxy_http_version 1.1;
    proxy_set_header Upgrade $http_upgrade;
    proxy_set_header Connection $connection_upgrade;
    proxy_set_header Host $http_host;
    proxy_pass http://grafana;
  }
}
```

### Configure HAProxy

To configure HAProxy to serve Grafana under a _sub path_:

```
frontend http-in
  bind *:80
  use_backend grafana_backend if { path /grafana } or { path_beg /grafana/ }

backend grafana_backend
  # Requires haproxy >= 1.6
  http-request set-path %[path,regsub(^/grafana/?,/)]

  # Works for haproxy < 1.6
  # reqrep ^([^\ ]*\ /)grafana[/]?(.*) \1\2

  server grafana localhost:3000
```

### Configure IIS <a href="#configure-iis" id="configure-iis"></a>

{% hint style="warning" %}
IIS requires that the URL Rewrite module is installed.
{% endhint %}

To configure IIS to serve Grafana under a _sub path_, create an Inbound Rule for the parent website in IIS Manager with the following settings:

* pattern: `grafana(/)?(.*)`
* check the `Ignore case` checkbox
* rewrite URL set to `http://localhost:3000/{R:2}`
* check the `Append query string` checkbox
* check the `Stop processing of subsequent rules` checkbox

This is the rewrite rule that is generated in the `web.config`:

```
  <rewrite>
      <rules>
          <rule name="Grafana" enabled="true" stopProcessing="true">
              <match url="grafana(/)?(.*)" />
              <action type="Rewrite" url="http://localhost:3000/{R:2}" logRewrittenUrl="false" />
          </rule>
      </rules>
  </rewrite>
```

Check the [tutorial on IIS URL Rewrites](https://grafana.com/tutorials/iis/) for more in-depth instructions.

### Configure Traefik

[Traefik](https://traefik.io/traefik/) Cloud Native Reverse Proxy / Load Balancer / Edge Router

Using the docker provider the following labels will configure the router and service for a domain or subdomain routing.

```
  labels:
      traefik.http.routers.grafana.rule: Host(`grafana.example.com`)
      traefik.http.services.grafana.loadbalancer.server.port: 3000
```

To deploy a **`subpath`**

```yaml
  labels:
      traefik.http.routers.grafana.rule: Host(`example.com`) && PathPrefix(`/grafana`)
      traefik.http.services.grafana.loadbalancer.server.port: 3000
```

Examples using the file provider,

```
http:
  routers:
    grafana:
      rule: Host(`grafana.example.com`)
      service: grafana
  services:
    grafana:
      loadBalancer:
        servers:
          - url: http://192.168.30.10:3000
```

```
http:
  routers:
    grafana:
      rule: Host(`example.com`) && PathPrefix(`/grafana`)
      service: grafana
  services:
    grafana:
      loadBalancer:
        servers:
          - url: http://192.168.30.10:3000
```
