# Router Service
---

### Usage

This entry will deploy a global load balancing service and expose the specified ports. When used with HTTP/HTTPS, you can greatly simplifiy port management to the cluster.  

### Setup

The hosts in the environment should all be behind a physical or cloud based load balancer. A common usecase would be to run in an Auto-Scaling Group behind an ELB or ALB balancer. An administrator of the environment would setup a wild card DNS entry pointed at the load balancer IP address. This load balancer must be configured to pass on Host Header or SNI host information headers. With ELB you will need to enable Proxy Protocol.

### Exposing a service

In order to expose a service on one of the router port, the user adds some configuration to their compose files.

```YAML
version: '2'
services:
  image: myimage:latest
  labels:
    enable: '80' # Port on the Router to join
  lb_config:
    port_rules:
      - hostname: "myservice.mydomain.com" # The domain name should be the wildcard domain name
        target_port: "80" # Port inside the container to expose
        path: "/url/path" # Optional incase you want to do path based routing.
```

In the above example `myservice.mydomain.com` would be routed on port `80` to container port `80`.

