/////////////////
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
    "x-request-id": "615c2d63-8076-97dc-b162-8ef24dc6be9a",
    "content-length": "0",
    "x-b3-traceid": "57b176ba1339f7cd7295e8ee765d9d74",
    "x-b3-spanid": "2977b10d820b7064",
    "x-b3-parentspanid": "7295e8ee765d9d74",
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
}tryc2@ip-172-31-0-31:~/tutorial-istio-envoy-lua-filters/example-6-istio/bk_sidecar/b4$ k get pods -n vr
NAME                         READY   STATUS    RESTARTS   AGE
jnginxvr-5c6b57df4b-bfq8b    2/2     Running   0          162m
jnginxvr2-84d9d8c557-hkkzm   2/2     Running   0          4m52s
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
    "x-request-id": "4ca9b724-23c6-9d4a-8068-b433e5c69c29",
    "tenant_id": "vr",
    "content-length": "0",
    "x-b3-traceid": "9307997f009ce0943b299e1d3f3fc46c",
    "x-b3-spanid": "f9132abe606c486a",
    "x-b3-parentspanid": "3b299e1d3f3fc46c",
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


/////////////
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
    "x-request-id": "5aa83993-1e7f-93e7-8b52-05c965424571",
    "content-length": "0",
    "x-b3-traceid": "e760d84507f86ed2924d8750604eb0c9",
    "x-b3-spanid": "d946f4fbeecc7a20",
    "x-b3-parentspanid": "924d8750604eb0c9",
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
}tryc2@ip-172-31-0-31:~/tutorial-istio-envoy-lua-filters/example-6-istio$ k get pods -n ll
NAME                         READY   STATUS    RESTARTS   AGE
jnginxll-76474cf48b-vhvkv    2/2     Running   0          161m
jnginxll2-6fb56688bf-8vnln   2/2     Running   0          3m57s
tryc2@ip-172-31-0-31:~/tutorial-istio-envoy-lua-filters/example-6-istio$ k exec -ti jnginxll2-6fb56688bf-8vnln -n ll -- curl web-service.default
Defaulting container name to jnginxll2.
Use 'kubectl describe pod/jnginxll2-6fb56688bf-8vnln -n ll' to see all of the containers in this pod.
{
  "path": "/",
  "headers": {
    "host": "web-service.default",
    "user-agent": "curl/7.52.1",
    "accept": "*/*",
    "x-forwarded-proto": "http",
    "x-request-id": "56618d01-d65f-9687-9e73-6b6a1c50c967",
    "tenant_id": "ll",
    "content-length": "0",
    "x-b3-traceid": "38644bfdf0bcd0f1d20200fe12cf8dde",
    "x-b3-spanid": "3ebbc3fb19dd3ed7",
    "x-b3-parentspanid": "d20200fe12cf8dde",
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
