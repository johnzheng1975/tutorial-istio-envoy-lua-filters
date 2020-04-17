/////////////
tryc2@ip-172-31-0-31:~/tutorial-istio-envoy-lua-filters/example-6-istio/bk_sidecar/b3$ k exec -ti jnginxll-76474cf48b-vhvkv -n ll -- curl web-service.default 
Defaulting container name to jnginxll.
Use 'kubectl describe pod/jnginxll-76474cf48b-vhvkv -n ll' to see all of the containers in this pod.
{
  "path": "/",
  "headers": {
    "host": "web-service.default",
    "user-agent": "curl/7.52.1",
    "accept": "*/*",
    "x-forwarded-proto": "http",
    "x-request-id": "638b2325-563e-92a4-acd3-301b5fed509d",
    "tenant_id": "ll",
    "content-length": "0",
    "x-b3-traceid": "36f3669973e39524cd455463f1afca67",
    "x-b3-spanid": "c680126651ba97c8",
    "x-b3-parentspanid": "cd455463f1afca67",
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

//////////////////
tryc2@ip-172-31-0-31:~/tutorial-istio-envoy-lua-filters/example-6-istio$ k exec -ti jnginxvr-5c6b57df4b-bfq8b  -n vr -- curl web-service.default
Defaulting container name to jnginxvr.
Use 'kubectl describe pod/jnginxvr-5c6b57df4b-bfq8b -n vr' to see all of the containers in this pod.
{
  "path": "/",
  "headers": {
    "host": "web-service.default",
    "user-agent": "curl/7.52.1",
    "accept": "*/*",
    "x-forwarded-proto": "http",
    "x-request-id": "65857180-f72b-9161-8b4a-cd5084261fd8",
    "tenant_id": "vr",
    "content-length": "0",
    "x-b3-traceid": "4982d8cad10303866caff2419f7f89f1",
    "x-b3-spanid": "c160188fa4c3b411",
    "x-b3-parentspanid": "6caff2419f7f89f1",
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
