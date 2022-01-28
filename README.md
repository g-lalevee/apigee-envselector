# apigee-envselector

On Apigee X/hybrid, to work around the limit of the number of deployable proxies per environment, you can attach several environments to the same environment group. See [About environments and environment groups](https://cloud.google.com/apigee/docs/api-platform/fundamentals/environments-overview).

Targeting a group of environments, envselector dynamically select in which attached environment to deploy your proxies.

2 strategies allow you to select the target environment:

- fulfill: deploy up to 50 proxies in the first environment, then move to the next one
- spread: deploy proxies uniformly to all environment in the given environment group 

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
    fulfilll
    spread
  Options:
    -g,--envgroup, Apigee environment group name
    -o,--organization, Apigee organization name
    -t,--token, GCP token 
    --debug, show verbose debug output
```

Example:

```shell
envSelector spread -o myOrg -g myGroup -t $TOKEN --debug

[DEBUG] Selecting target environment to deploy proxy in environment attached to environment group "myGroup", with strategy "fulfill"...
[DEBUG] dev3 -> 0
[DEBUG] dev2 -> 0
[DEBUG] dev1 -> 42
[DEBUG] Selected environment: dev1

configurable-dev
```