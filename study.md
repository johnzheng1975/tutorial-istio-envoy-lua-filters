#

## Example of istio
example-6-istio

### Change code for debug:
```
local lua_value2 = {locale="mylvalue"}
request_handle:headers():replace("locale",lua_value2.locale)
```

### LUA Programming
- https://www.lua.org/pil/contents.html#1

### Onlline check
- https://www.lua.org/cgi-bin/demo

### Istio proxy debug
```
request_handle:logInfo("******* enter envoy_on_request")
```

### Change log level of istio-proxy
Refer https://www.envoyproxy.io/docs/envoy/latest/operations/admin#get--server_info
```
curl localhost:15000/server_info
curl -X POST localhost:15000/logging?level=info
```

### Normal command
```
 1291  vi envoy-filter.yaml 
 1292  k replace -f envoy-filter.yaml --force
 1272  k delete pods -n is istio-ingressgateway-9cdf984d4-9f8v5
 1273  k get pods -n is
 1276  k exec -ti -n is  istio-ingressgateway-9cdf984d4-m8gfv -- curl -X POST localhost:15000/logging?level=info
 1277  k logs -n is  istio-ingressgateway-9cdf984d4-m8gfv -f
```

### Change for sidecar
For configure
```
apiVersion: networking.istio.io/v1alpha3
kind: EnvoyFilter
metadata:
  name: custom-filter-2
  namespace: istio-system
spec:
  filters:
  - listenerMatch:
      portNumber: 80
      listenerType: SIDECAR_INBOUND
      listenerProtocol: HTTP
    filterName: envoy.lua
    filterType: HTTP
    filterConfig:
      inlineCode: |
        function envoy_on_request(request_handle)
          request_handle:logInfo("******* enter envoy_on_request custom-filter-2")
          request_handle:headers():replace("mylol","hivalue")
        end
```

Test result:
```
tryc2@ip-172-31-0-31:~/tutorial-istio-envoy-lua-filters/example-6-istio/bk_sidecar$ k exec -ti nginx-c46747ffd-bnjgg -- curl web-service.default
Defaulting container name to nginx.
Use 'kubectl describe pod/nginx-c46747ffd-bnjgg -n default' to see all of the containers in this pod.
{
  "path": "/",
  "headers": {
    "host": "web-service.default",
    "user-agent": "curl/7.52.1",
    "accept": "*/*",
    "x-forwarded-proto": "http",
    "x-request-id": "4d2a5cd5-4623-9783-bd57-498f5c363bb9",
    "content-length": "0",
    "mylol": "hivalue",
    "x-b3-traceid": "a6762777a778d56fb2be06d83a4d50fa",
    "x-b3-spanid": "9b6d418dbd6dffb8",
    "x-b3-parentspanid": "b2be06d83a4d50fa",
    "x-b3-sampled": "1"
  },
  "method": "GET",
  "body": "",
  "fresh": false,
  "hostname": "web-service.default",
  "ip": "::ffff:127.0.0.1",
  "ips": [],
  "protocol": "http",
  "query": {},
  "subdomains": [],
  "xhr": false,
  "os": {
    "hostname": "web-74f84ccc8-86vbr"
  }
}
```
