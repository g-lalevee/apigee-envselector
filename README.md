# apigee-envselector


Apigee X/hybrid has a lim
The Apigee Environment Selector lets you find the nevironment 

On Apigee X/hybrid, to work around the limits of the number of deployable proxies per environment, you can associate several environments to the same environment group. See [About environments and environment groups](https://cloud.google.com/apigee/docs/api-platform/fundamentals/environments-overview).

Targeting a group of environments, envselector allows you to dynamically select in which attached environment to deploy your proxies.

2 strategies allow you to select the target environment:

- fulfill: deploy up to 50 proxies in the first environment, then move to the next one
- spread: deploy proxies uniformly to all environment in the given environment group 

Please note that apigee-envselector works only with Apigee X and Apigee hybrid.

To use it as a CLI you can add it to your path:

```sh
PATH="$PATH:$PWD/bin"
```

## General Usage

```text
$ envselector help

Apigee Environment Selector.
usage: envselector STRATEGY -e ENV -o ORG [-t TOKEN]
  Strategies:
     fullfill ()
     spread
  Options:
  -g,--envgroup, Apigee environment group name
  -o,--organization, Apigee organization name
  -t,--token, GCP token 
  --debug, show verbose debug output
```
