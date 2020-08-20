+++
title = "malicious"
description = "*malicious* emits logs and Prometheus metrics when a malicious domain is requested."
weight = 10
tags = [  "plugin" , "malicious" ]
categories = [ "plugin", "external" ]
date = "2020-08-18T18:00:00+01:00"
repo = "https://github.com/giantswarm/coredns-malicious-domain-plugin"
home = "https://github.com/giantswarm/coredns-malicious-domain-plugin/blob/master/README.md"
+++

## Description

The *malicious* (domain) plugin accepts a list of malicious domains and emits a log entry and Prometheus metrics when one is requested.

Malicious domains can be loaded from a local file or a URL and can be automatically reloaded after a specified period.

If the `prometheus` plugin is also enabled, this plugin emits the following metrics:

- `malicious_domains_hits_total{server, requestor, domain}` - counts the number of malicious domains requested, including the IP of the requesting host and the exact domain
- `malicious_domains_failed_reloads_count{server}` - counts the number of times the plugin has failed to reload
- `malicious_domains_cache_check_duration_seconds{server}` - summary for determining the average time it takes to check the cache
- `malicious_domains_blacklisted_items_count{server}` - current number of domains stored in the list

See the project Readme for additional details.

## Syntax

The malicious plugin takes 4 arguments:

- the source type for the domain list: either `url` or `file`
- the path to the source: either a url or file path
- the format of the file to expect: either `hostfile` or `text`
- the reload period: an optional Go Duration after which the list will be regenerated*

\* when automatically reloading from a URL, please be friendly to the service hosting the file.

```text
malicious {
    <source type> <source path> <file format>
    reload <reload period>
}
```

## Example

Using a `hostfile`-formatted file from a URL:

```text
malicious {
    url https://urlhaus.abuse.ch/downloads/hostfile/ hostfile
    reload 30m
}
```

Using a local, `text`-formatted file:

```text
malicious {
    file domains.txt text
    reload 5m
}
```
