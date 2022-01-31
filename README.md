# apigee-envselector

On Apigee X/hybrid, to work around the limit of the number of deployable proxies per environment, you can attach several environments to the same environment group. See [About environments and environment groups](https://cloud.google.com/apigee/docs/api-platform/fundamentals/environments-overview).

Targeting a group of environments, envselector dynamically select in which attached environment to deploy your proxies.

2 strategies allow you to select the target environment:

- fill-first: deploy up to 50 proxies in the first environment, then move to the next one
- spread-out: deploy proxies uniformly to all environment in the given environment group 

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

Apigee Environment Selector.
usage: envselector STRATEGY -e ENV -o ORG [-t TOKEN]
  STRATEGY:
    fill-first
    spread-out
  Options:
    -g,--envgroup, Apigee environment group name
    -o,--organization, Apigee organization name
    -t,--token, GCP token 
    --debug, show verbose debug output
    --debug-all, show verbose debug output, including Sackmesser
```

## Example:

```shell

$ TOKEN=$(gcloud auth print-access-token)

$ envSelector fill-first -o myOrg -g myGroup -t $TOKEN --debug
[DEBUG] Selecting target environment in list of environments attached to environment group "myGroup", with strategy "fill-first"...
[DEBUG]    dev3 -> 0
[DEBUG]    dev2 -> 0
[DEBUG]    dev1 -> 42
[DEBUG]
[DEBUG] Selected environment: dev1
dev1

$ envSelector spread-out -o myOrg -g myGroup -t $TOKEN 
dev2

$ envSelector update -o myOrg -g myGroup -p myProxy -t $TOKEN --debug
[DEBUG] Selecting target environment in list of environments attached to environment group "myGroup", with strategy "update"...
[DEBUG]    dev3 -> 0
[DEBUG]    dev2 -> 0
[DEBUG]    dev1 -> 42
[DEBUG] 
[DEBUG] Searching for the environment in which the latest proxy release was deployed...
[DEBUG]    dev3 -> myProxy not deployed
[DEBUG]    dev2 -> myProxy not deployed
[DEBUG]    dev1 -> myProxy, release 3
[DEBUG] myProxy last release (3) is deployed in dev1
dev1

```