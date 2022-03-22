# apigee-envselector

**This is not an official Google product.**<BR>This implementation is not an official Google product, nor is it part of an official Google product. Support is available on a best-effort basis via GitHub.

***
  
On Apigee X/hybrid, to work around the limit of the number of deployable proxies per environment, you can attach several environments to the same environment group. See [About environments and environment groups](https://cloud.google.com/apigee/docs/api-platform/fundamentals/environments-overview).

Targeting a group of environments, envselector dynamically select in which attached environment to deploy your proxies.

2 strategies allow you to select the target environment:

- fill-first: deploy up to 50 proxies in the first environment, then move to the next one
- spread-out: deploy proxies uniformly to all environment in the given environment group 

If you add --proxy option, envselector will try first to retrieve the environment in which the latest given proxy release was deployed, to reuse this environment.


---
**Notes**

- apigee-eenvselector requires apigee-sackmesser. See [GitHub apigee/devrel/apigee-sackmesser](https://github.com/apigee/devrel/tree/main/tools/apigee-sackmesser).

- apigee-envselector works only with Apigee X and Apigee hybrid.

---

To use it as a CLI you can add it to your path:

```sh
PATH="$PATH:$PWD/bin"
```

## General Usage

```text
$ envselector help

pigee Environment Selector.
usage: envselector STRATEGY -e ENV -o ORG [-p PROXY_NAME] [-t TOKEN] [options]
  STRATEGY:
    fill-first
    spread-out
  Options:
    -g,--envgroup, Apigee environment group name
    -o,--organization, Apigee organization name
    -p,--proxy, Apigee proxy name to check deployment
    -t,--token, GCP token 
    --debug, show verbose debug output
    --debug-all, show verbose debug output, including Sackmesser
```

## Example:

```shell

$ TOKEN=$(gcloud auth print-access-token)

$ envSelector fill-first -o myOrg -g myGroup -t $TOKEN --debug
[DEBUG] Selecting target environment in list of environments attached to environment group "myGroup", with strategy "fill-first"...
[DEBUG] Getting deployments/environment...
[DEBUG]    dev3 -> 0
[DEBUG]    dev2 -> 0
[DEBUG]    dev1 -> 42
[DEBUG]
[DEBUG] Selected environment: dev1
dev1

$ envSelector spread-out -o myOrg -g myGroup -t $TOKEN 
dev2

[deploy myProxy in dev2]

$  ./envSelector spread-out -o myOrg -g myGroup -p myProxy -t $TOKEN --debug 
[DEBUG] Selecting target environment in list of environments attached to environment group "myGroup", with strategy "spread-out"...
[DEBUG] Getting deployments/environment...
[DEBUG]    dev3 -> 0
[DEBUG]    dev2 -> 1
[DEBUG]    dev1 -> 42
[DEBUG] 
[DEBUG] Searching for the environment in which the latest proxy release was deployed...
[DEBUG]    dev3 -> myProxy not deployed
[DEBUG]    dev2 -> myProxy, release 3
[DEBUG]    dev1 -> myProxy not deployed
[DEBUG] myProxy last release (3) is deployed in dev2
[DEBUG] 
[DEBUG] Selected environment: dev2
dev2

```
