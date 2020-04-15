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
