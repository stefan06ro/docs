---
# Generated by scripts/aggregate-changelogs. WARNING: Manual edits to this files will be overwritten.
aliases:
- /changes/tenant-cluster-releases-azure/releases/azure-v14.1.6/
changes_categories:
- Workload cluster releases for Azure
changes_entry:
  repository: giantswarm/releases
  url: https://github.com/giantswarm/releases/tree/master/azure/archived/v14.1.6
  version: 14.1.6
  version_tag: v14.1.6
date: '2021-05-14T11:36:03+00:00'
description: Release notes for Azure workload cluster release v14.1.6, published on
  14 May 2021, 11:36
title: Workload cluster release v14.1.6 for Azure
---

This release downgrades Flatcar Container Linux in order to debug connectivity issues. Please do not upgrade to this release unless it is necessary. Reach out to your Solution Engineer to validate whether this release applies to your solution.

## Change details


### containerlinux [2605.12.0](https://www.flatcar-linux.org/releases/#release-2605.12.0)

Reverted flatcar to previous version.
