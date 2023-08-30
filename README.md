# Crossplane configuration for "EKS as a Service"

This repository contains the definition for a [Crossplane configuration](https://docs.crossplane.io/v1.11/concepts/packages/#configuration-packages) that bundles a set of API definitions. This configuration is a starting point for new users who are creating their first control plane in [Upbound](https://console.upbound.io).

When this configuration is installed on a control plane, the control plane will have APIs to provision fully configured Amazon Elastic Kubernetes Service (EKS) clusters with secure networking, composed using cloud service primitives from the [Upbound Official AWS Provider](https://marketplace.upbound.io/providers/upbound/provider-aws). App deployments can securely connect to the infrastructure they need using secrets distributed directly to the app namespace.

## What's Inside

A custom API in [Crossplane](https://docs.crossplane.io/v1.11/getting-started/introduction/) is defined by:

- a CompositeResourceDefinition (XRD). This defines the schema or shape of the API.
- A Composition(s). Compositions implement the schema by _composing_ a set of Crossplane managed resources together.

For this configuration, the EKS API is defined by:

- a [KubernetesCluster](/apis/definition.yaml) type
- the KubernetesCluster is composed of an [XNetwork](/apis/network/definition.yaml) and [XEKS](/apis/eks/definition.yaml) types.
- the XNetwork is [composed](/apis/network/composition.yaml) of a the following resources: a VPC, InternetGateway, 4 Subnets, a RouteTable, Routes, 5 RouteTableAssociations, a SecurityGroup, and 2 security group role.
- the XEKS is [composed](/apis/eks/composition.yaml) of the following resources: Cluster, a NodeGroup, an OpenIDConnectProvider, 2 Roles, 4 RolePolicyAttachments, and ClusterAuth.

This repository also contains an [example claim](/.up/examples/cluster.yaml). You can apply this file on your control plane to invoke the EKS API and cause a cluster to be created.

## Next Steps

This repository is a starting point. You should be feel encouraged to:

1) create new API definitions in this same repo
2) tweak the existing API definition for EKS to your needs

Upbound will automatically detect the commits you make in your repo and build the configuration package for you. To learn more about how to build APIs for your managed control planes in Upbound, read the guide on [Upbound's docs](https://docs.upbound.io).
