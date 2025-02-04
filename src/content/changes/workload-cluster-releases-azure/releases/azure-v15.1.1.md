---
# Generated by scripts/aggregate-changelogs. WARNING: Manual edits to this files will be overwritten.
aliases:
- /changes/tenant-cluster-releases-azure/releases/azure-v15.1.1/
changes_categories:
- Workload cluster releases for Azure
changes_entry:
  repository: giantswarm/releases
  url: https://github.com/giantswarm/releases/tree/master/azure/archived/v15.1.1
  version: 15.1.1
  version_tag: v15.1.1
date: '2021-07-22T08:54:51+00:00'
description: Release notes for Azure workload cluster release v15.1.1, published on
  22 July 2021, 08:54
title: Workload cluster release v15.1.1 for Azure
---

This is a bugfix release that solves an issue preventing clusters to be successfully created on new organizations.

There is no need to upgrade workload clusters running the 15.1.0 release to this one because there are no changes in the
workload cluster components.

## Change details

### azure-operator [5.8.1](https://github.com/giantswarm/azure-operator/releases/tag/v5.8.1)

#### Fixed
- Fix namespace in secret reference of `AzureClusterIdentity`.
