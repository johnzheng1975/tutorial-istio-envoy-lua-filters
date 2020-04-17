## Requirment
Add http header when pods access others; Different namespace add different value.

That's say, you has two namespace, one is ll, another is vr. Both are istio injected. 
- For the pods with label app=jn2 in namespace vr, when it access other pods, need add header "tenant_id: vr"
- For the pods with label app=jn2 in namespace ll, when it access other pods, need add header "tenant_id: ll"

## Prepare
Here are env prepared for testing.
```
 kubectl create ns vr
 kubectl create ns ll
 kubectl label namespace vr istio-injection=enabled
 kubectl label namespace ll istio-injection=enabled
 kubectl run jnginxvr --image=johnzheng/nginx --labels "app=jn1,tenantid=vr" -n vr
 kubectl run jnginxll --image=johnzheng/nginx --labels "app=jn1,tenantid=ll" -n ll
 kubectl run jnginxvr2 --image=johnzheng/nginx --labels "app=jn2,tenantid=vr" -n vr
 kubectl run jnginxll2 --image=johnzheng/nginx --labels "app=jn2,tenantid=ll" -n ll
```

## Envoy Filter
- envoy filter in vr namespace
```
apiVersion: networking.istio.io/v1alpha3
kind: EnvoyFilter
metadata:
  name: efvr1
  namespace: vr
spec:
  workloadSelector:
    labels:
      app: jn2
  configPatches:
    # The first patch adds the lua filter to the listener/http connection manager
  - applyTo: HTTP_FILTER
    match:
      context: SIDECAR_OUTBOUND
      listener:
        portNumber: 80
        filterChain:
          filter:
            name: "envoy.http_connection_manager"
            subFilter:
              name: "envoy.router"
    patch:
      operation: INSERT_BEFORE
      value: # lua filter specification
       name: envoy.lua
       config:
         inlineCode: |
           function envoy_on_request(request_handle)
             request_handle:logInfo("******* enter envoy_on_request ef-vr")
             request_handle:headers():replace("tenant_id","vr")
           end
```

- envoy filter in ll namespace
```
apiVersion: networking.istio.io/v1alpha3
kind: EnvoyFilter
metadata:
  name: efll1
  namespace: ll
spec:
  workloadSelector:
    labels:
      app: jn2
  configPatches:
    # The first patch adds the lua filter to the listener/http connection manager
  - applyTo: HTTP_FILTER
    match:
      context: SIDECAR_OUTBOUND
      listener:
        portNumber: 80
        filterChain:
          filter:
            name: "envoy.http_connection_manager"
            subFilter:
              name: "envoy.router"
    patch:
      operation: INSERT_BEFORE
      value: # lua filter specification
       name: envoy.lua
       config:
         inlineCode: |
           function envoy_on_request(request_handle)
             request_handle:logInfo("******* enter envoy_on_request ef-ll")
             request_handle:headers():replace("tenant_id","ll")
           end 
```

## Test Result
### Test in VR namespace
```
tryc2@ip-172-31-0-31:~/tutorial-istio-envoy-lua-filters/example-6-istio/bk_sidecar/b4$ k exec -ti jnginxvr-5c6b57df4b-bfq8b  -n vr -- curl web-service.default
Defaulting container name to jnginxvr.
Use 'kubectl describe pod/jnginxvr-5c6b57df4b-bfq8b -n vr' to see all of the containers in this pod.
{
  "path": "/",
  "headers": {
    "host": "web-service.default",
    "user-agent": "curl/7.52.1",
    "accept": "*/*",
    "x-forwarded-proto": "http",
    "x-request-id": "f6f2c3ac-b979-95d5-8ca0-b5ed3c0ceeb8",
    "content-length": "0",
    "x-b3-traceid": "9fcd6123dd29b4bf0ca9f7a7123028ac",
    "x-b3-spanid": "998c19c2f063b5c3",
    "x-b3-parentspanid": "0ca9f7a7123028ac",
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
    "hostname": "web-74f84ccc8-544f8"
  }
}tryc2@ip-172-31-0-31:~/tutorial-istio-envoy-lua-filters/example-6-istio/bk_sidecar/b4$ 
tryc2@ip-172-31-0-31:~/tutorial-istio-envoy-lua-filters/example-6-istio/bk_sidecar/b4$ k exec -ti jnginxvr2-84d9d8c557-hkkzm -n vr -- curl web-service.default
Defaulting container name to jnginxvr2.
Use 'kubectl describe pod/jnginxvr2-84d9d8c557-hkkzm -n vr' to see all of the containers in this pod.
{
  "path": "/",
  "headers": {
    "host": "web-service.default",
    "user-agent": "curl/7.52.1",
    "accept": "*/*",
    "x-forwarded-proto": "http",
    "x-request-id": "95c677e3-c35a-9008-9623-3e34288b424a",
    "tenant_id": "vr",
    "content-length": "0",
    "x-b3-traceid": "b693bf0eb776f0b45214d7e18676e813",
    "x-b3-spanid": "79b2b43bd61f5d02",
    "x-b3-parentspanid": "5214d7e18676e813",
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
    "hostname": "web-74f84ccc8-544f8"
  }
```

### Test in ll namespace
```
tryc2@ip-172-31-0-31:~/tutorial-istio-envoy-lua-filters/example-6-istio$ k exec -ti jnginxll-76474cf48b-vhvkv -n ll -- curl web-service.default
Defaulting container name to jnginxll.
Use 'kubectl describe pod/jnginxll-76474cf48b-vhvkv -n ll' to see all of the containers in this pod.
{
  "path": "/",
  "headers": {
    "host": "web-service.default",
    "user-agent": "curl/7.52.1",
    "accept": "*/*",
    "x-forwarded-proto": "http",
    "x-request-id": "5f057501-42d4-9093-9a2a-d77d46cd3d4d",
    "content-length": "0",
    "x-b3-traceid": "67b7d5db0c1cbffe8195be2b76a03fca",
    "x-b3-spanid": "39d4a868084dbdd4",
    "x-b3-parentspanid": "8195be2b76a03fca",
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
    "hostname": "web-74f84ccc8-544f8"
  }
}tryc2@ip-172-31-0-31:~/tutorial-istio-envoy-lua-filters/example-6-istio$ k exec -ti jnginxll2-6fb56688bf-8vnln -n ll -- curl web-service.default
Defaulting container name to jnginxll2.
Use 'kubectl describe pod/jnginxll2-6fb56688bf-8vnln -n ll' to see all of the containers in this pod.
{
  "path": "/",
  "headers": {
    "host": "web-service.default",
    "user-agent": "curl/7.52.1",
    "accept": "*/*",
    "x-forwarded-proto": "http",
    "x-request-id": "7ef6f9ad-853c-9c67-bfe5-bb23bb293c2d",
    "tenant_id": "ll",
    "content-length": "0",
    "x-b3-traceid": "b3f2a6ab7b73d3178f0a813ab8721e95",
    "x-b3-spanid": "b2af68727fa98493",
    "x-b3-parentspanid": "8f0a813ab8721e95",
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
    "hostname": "web-74f84ccc8-544f8"
  }
}
```

