---
linkTitle: login
title: "'kubectl gs login' command reference"
description: Reference documentation on how to ensure an authenticated kubectl context for a Giant Swarm management cluster, using 'kubectl gs'.
weight: 60
menu:
  main:
    identifier: uiapi-kubectlgs-login
    parent: uiapi-kubectlgs
aliases:
  - /reference/kubectl-gs/login/
owner:
  - https://github.com/orgs/giantswarm/teams/team-rainbow
user_questions:
  - How can I log in with kubectl for the Management API?
last_review_date: 2021-01-01
---

# `kubectl gs login`

This command allows to ensure an authenticated kubectl context is selected.

## Usage

The command is called with the general syntax

```nohighlight
kubectl gs login [argument]
```

where `argument` can be either:

- **Empty:** If the current kubectl context is a Giant Swarm management cluster, this ensures that the OIDC auth token will be refreshed and show the name of the current context.

- **Web interface URL:** The URL of the management cluster's [web UI]({{< relref "/ui-api/web/" >}}) instance.

- **Management API endpoint:** The URL of the management cluster's Kubernetes API endpoint.

- **Context name:** Name of a Giant Swarm management cluster kubectl context, with or without `gs-` prefix.

## Examples

Using the Web URL as an argument:

```nohighlight
kubectl gs login https://happa.g8s.gattaca.westeurope.azure.gigantic.io
```
