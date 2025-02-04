---
linkTitle: template nodepool
title: "'kubectl gs template nodepool' command reference"
description: Reference documentation on how to create a manifest for a node pool using 'kubectl gs'.
weight: 100
menu:
  main:
    parent: uiapi-kubectlgs
aliases:
  - /reference/kubectl-gs/template-nodepool/
last_review_date: 2021-07-08
owner:
  - https://github.com/orgs/giantswarm/teams/team-rainbow
user_questions:
  - How can I create a node pool manifest for the Management API?
---

# `kubectl gs template nodepool`

Node pools are groups of worker nodes sharing common configuration. In terms of custom resources they consist of custom resources of type

The outcome depends on the provider, set via the `--provider` flag:

For AWS (`--provider aws`):

- [`MachineDeployment`]({{< relref "/ui-api/management-api/crd/machinedeployments.cluster.x-k8s.io.md" >}}) (API version `cluster.x-k8s.io/v1alpha2`)
- [`AWSMachineDeployment`]({{< relref "/ui-api/management-api/crd/awsmachinedeployments.infrastructure.giantswarm.io.md" >}}) (API version `infrastructure.giantswarm.io/v1alpha2`)

For Azure (`--provider azure`):

- [`MachinePool`]({{< relref "/ui-api/management-api/crd/machinepools.exp.cluster.x-k8s.io.md" >}}) (API version `cluster.x-k8s.io/v1alpha3`)
- [`AzureMachinePool`]({{< relref "/ui-api/management-api/crd/azuremachinepools.exp.infrastructure.cluster.x-k8s.io.md" >}}) (API version `exp.infrastructure.cluster.x-k8s.io/v1alpha3`)
- [`Spark`]({{< relref "/ui-api/management-api/crd/sparks.core.giantswarm.io.md" >}}) (API version `core.giantswarm.io/v1alpha1`)

## Usage

To create the manifests for a new node pool, use this command:

```nohighlight
kubectl gs template nodepool
```

Here are the supported flags:

- `--provider` - The infrastructure provider, either `aws` or `azure`.
- `--availability-zones` - list of availability zones to use, instead of setting a number. Use comma to separate values. (e. g. `eu-central-1a,eu-central-1b`)
- `--cluster-name` - Unique identifier of the cluster the node pool should be added to.
- `--description` - User-friendly description of the purpose of the node pool. (default *Unnamed node pool*)
- `--nodes-max` - maximum number of worker nodes for the node pool. (default 10)
- `--nodes-min` - minimum number of worker nodes for the node pool. (default 3)
- `--output` - Sets a file path to write the output to. If not set, standard output will be used.
- `--organization` - Name of the organization owning the cluster.

### AWS specific

- `--aws-instance-type` - EC2 instance type to use for workers, e. g. *m5.2xlarge*. (default *m5.xlarge*)
- `--use-alike-instance-types` - Enables the use of instance types similar to the one specified via `--aws-instance-type` (default: false). This can increase the likelihood of getting the required instances, especially when requesting spot instances. See [our reference]({{< relref "/reference/similar-ec2-instance-types" >}}) for details.
- `--on-demand-percentage-above-base-capacity` - To use only on-demand instances, set this to 100. For any other value, the remainder to 100 will be filled with spot instances. For example, 50 will create a node pool that is half spot and half on-demand instances. 0 (zero) will use only spot instances. See [our AWS spot instances docs]({{< relref "/advanced/spot-instances/aws" >}}) for more information.
- `--on-demand-base-capacity` - Can be used to set a fixed number of on-demand instances, regardless of the percentage (see above) of spot vs. on-demand to be used otherwise.
- `--machine-deployment-subnet`: Size of the IPv4 subnet to reserve for the node pool. Must be a number between 20 and 28. For example, 24 stands for a /24 subnet with 256 addresses. Check the [`alpha.aws.giantswarm.io/aws-subnet-size`]({{< relref "/ui-api/management-api/crd/awsmachinedeployments.infrastructure.giantswarm.io.md#v1alpha2-alpha.aws.giantswarm.io/aws-subnet-size" >}}) annotation for details.

### Azure specific

- `--azure-vm-size` - Azure VM size to use for workers (e.g. *Standard_D4s_v3*).
- `--azure-spot-vms` - Whether to use spot VMs for this node pool (defaults to false which means not to use spot VMs).
- `--azure-spot-vms-max-price` - Max hourly price in USD to pay for one spot VM. If the current price for the VM is exceeded, the VM is deleted. If not set, the on-demand price for the same machine size is used as limit.

## Example

```nohighlight
kubectl gs template nodepool \
  --provider aws \
  --cluster-name a1b2c \
  --description "General purpose" \
  --availability-zones eu-central-1a \
  --organization acme \
  --aws-instance-type m5.4xlarge
```

## Output

The above example command would generate the following output:

```yaml
apiVersion: cluster.x-k8s.io/v1alpha2
kind: MachineDeployment
metadata:
  annotations:
    giantswarm.io/docs: https://docs.giantswarm.io/reference/cp-k8s-api/machinedeployments.cluster.x-k8s.io
  creationTimestamp: null
  labels:
    cluster-operator.giantswarm.io/version: ""
    giantswarm.io/cluster: a1b2c
    giantswarm.io/machine-deployment: fo2xh
    giantswarm.io/organization: acme
    release.giantswarm.io/version: ""
  name: fo2xh
  namespace: default
spec:
  selector: {}
  template:
    metadata: {}
    spec:
      bootstrap: {}
      infrastructureRef:
        apiVersion: infrastructure.giantswarm.io/v1alpha2
        kind: AWSMachineDeployment
        name: fo2xh
        namespace: default
      metadata: {}
status: {}
---
apiVersion: infrastructure.giantswarm.io/v1alpha2
kind: AWSMachineDeployment
metadata:
  annotations:
    giantswarm.io/docs: https://docs.giantswarm.io/reference/cp-k8s-api/awsmachinedeployments.infrastructure.giantswarm.io
  creationTimestamp: null
  labels:
    aws-operator.giantswarm.io/version: ""
    giantswarm.io/cluster: a1b2c
    giantswarm.io/machine-deployment: fo2xh
    giantswarm.io/organization: acme
    release.giantswarm.io/version: ""
  name: fo2xh
  namespace: default
spec:
  nodePool:
    description: General purpose
    machine:
      dockerVolumeSizeGB: 100
      kubeletVolumeSizeGB: 100
    scaling:
      max: 10
      min: 3
  provider:
    availabilityZones:
    - eu-central-1a
    instanceDistribution:
      onDemandBaseCapacity: 0
      onDemandPercentageAboveBaseCapacity: 100
    worker:
      instanceType: m5.4xlarge
      useAlikeInstanceTypes: false
status:
  provider:
    worker: {}
```
