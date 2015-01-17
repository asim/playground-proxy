# Playground Proxy

The playground proxy is used as a fixed point of entry for external access to the playground apps. 
It watches the kubernetes api for service changes and creates nginx server configs for those set 
to be proxied. By default all apps created in the playground are proxied. Each service is given 
the hostname [servicename].PROXY_DOMAIN, an additional alias is created when the label hostname= 
is found in the kubernetes label set for the service.

## Getting started

### Env variables

The following variables need to be set at runtime:

- PROXY_NGINX_DIR - The location where the proxy should write nginx config e.g /etc/nginx/conf.d
- PROXY_DOMAIN - The domain to proxy for e.g. set as example.com, service records will be [service].example.com
- PROXY_LABEL - The label to identify apps which would be proxied e.g "proxy" will look for proxy=true

 These values will already be set when running the proxy inside kubernetes:
- KUBERNETES_RO_SERVICE_HOST - The kubernetes master read only host ip
- KUBERNETES_RO_SERVICE_PORT - The kubernetes master read only host port
- KUBERNETES_API_PROTOCOL - The kubernetes master protocol e.g http

### Running the proxy

Set the PROXY_DOMAIN in playground-proxy.json 
Set the publicIPs field in playground-proxy-service.json to the ips of the minions on which the proxy will run

Start the proxy
```
kubectl create -f playground-proxy.json
kubectl create -f playground-proxy-service.json
```

## Acknowledgements

Service watch code from SkyDNS.
