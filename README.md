# The project targets to create a basic authentication below

### Project Info
* Author (GitHub ID)
    * kumarvgit
* Version
    * 0.0.1

### Environment details
* Kong API Gateway - 2.1.X
* K8S (Kubernetes) - Minikube v1.16.0
* OS description
    * Distributor ID:	Ubuntu
    * Description:	Ubuntu Hirsute Hippo (development branch)
    * Release:	21.04
    * Codename:	hirsute

### Installation
* https://docs.konghq.com/2.2.x/kong-for-kubernetes/install/

### Additional commands
* Export kong proxy ip in minikube
    * ```export KONG_HTTP=$(minikube ip):$(kubectl -nkong get svc kong-proxy --no-headers | awk '{print $5}' | cut -d / -f 1 | cut -d : -f 2)```


### Create secret for KongConsumer
* Default namespace 
    * ```kubectl create secret generic harry-basicauth  --from-literal=kongCredType=basic-auth  --from-literal=username=harry --from-literal=password=foo```
* Kong namespace
    * ```kubectl create secret generic harry-basicauth  -nkong --from-literal=kongCredType=basic-auth  --from-literal=username=harry --from-literal=password=foo```

### Testing Basic auth (We need to pass two heasers)
* Browser
    * Authorization
        * in example harry:foo is thee secret so we need to pass "Authorization" header as "Basic   aGFycnk6Zm9v"
            * e.g. ``` Authorization: Basic aGFycnk6Zm9v```
    * Host
        * in example host is ``` api.gengo.dev ```
            * ``` Host: api.gengo.dev```

* Using cURL
    * ``` curl -i -u 'harry:foo' -H "Host: api.gengo.dev" $KONG_HTTP/ ```

### Output (part of the output)
* Without passing proper credentials
    * ``` Invalid authentication credentials.```
* With appropriate credentials
    * ``` Hostname: echo-5fc5b5bc84-p28sq```

## Primary References 
#### Discussions
* https://itnext.io/kong-gateway-in-kubernetes-ad86981f93a5
* https://github.com/Kong/kubernetes-ingress-controller/issues/808

#### Base64 encoding
* https://www.base64encode.org/