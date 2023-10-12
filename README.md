# Port and Upbound demo
This is a guide for preparing a demo environment to showcase Port + Upbound capabilities, both as a software catalog and self-service hub for developers, and a platform management tool for the platform team.

The final goal of this demo is to interact with Upbound using Port, and provide EKSaaS using Upbound's capabilities and control planes as a backend, while reporting data regarding deployed clusters back to Port.

All needed manifests and information is in this git repository.

# In this repository
## Upbound XR template
In `.upbound/examples/cluster.yaml` is the default example provided by the EKSaaS configuration. This file is later used as a template for creating new XRs in the Upbound control planes.

## Port assets
In this repository, there is a `.port/` directory ([link](https://github.com/port-demo/port-upbound-demo/tree/main/.port)). It holds manifests which will be used in this guide to create blueprints, and their corresponding Port actions.

## Github workflows - The Port actions backend
The `.github/workflows/` directory holds the necessary Github action definitions. These actions will be used as the backend for the different Port actions for interacting with Upbound.

The demo starts off on a completely clean slate - an empty Upbound organization, an empty git repository, and a clean Port environment.

# Prerequisites
## Upbound
Before following the guide, you will need to set up an Upbound organization, initialize it and keep track of some information:
- Save the `Organization ID` for later;
- Set up the default EKSaaS configuration in the Upbound organization;
- Create an API token and save it for later.

## Port 
It would be best to start off with a clean Port environment. Make sure that the Port organization used in the demo doesn't have any entities or blueprints.
Save the Port organization's `CLIENT_ID` and `CLIENT_SECRET` for later.

## Git repository
The demo, and the state of the different control planes will be handled in a github repository. 
Create a new git repository, and make sure the Port's Github app is installed on it either by:
- installing Port's github app on all the repositories in the used Github organization;
- or by adding the new repository to the allowed repository list of Port's Github app in the organization.


# Demo guide
After completing the [prerequisites](https://github.com/port-demo/port-upbound-demo/blob/main/README.md#prerequisites) step, you can start following the guide.

## Setting up the git repository
### Create all the necessary files and directory structure
- `.github/workflows/` - Copy all of the `.yaml` files from this repository, to the new one in the same directory hierarchy. 

    Please note that in `delete-cluster.yaml` and `apply-clusters.yaml`, there is an env var configured:
    ```bash
        env:
            UPBOUND_ORG_ID: port # To be changed
    ```
    The value for this environment variable needs to match the Upbound `Organization ID` we set aside earlier.

- `.up/clusters/` - Create this folder in the repository. It will hold subdirectories which will correspond to different Upbound control planes, and each subdirectory will hold all the XR manifests to be deployed in the specific control plane.

- `.up/examples/cluster.yaml` - Copy this file to the new repository in the same directory hierarchy.

### Create repository secrets for the Github actions to use
Follow Github's [guide](https://docs.github.com/en/actions/security-guides/using-secrets-in-github-actions#creating-secrets-for-a-repository) to add required secrets to that repository. These are the secrets the need to be created:
* `UPBOUND_TOKEN` - The Upbound organization's API token;
* `PORT_CLIENT_ID` - The Port organization's client id;
* `PORT_CLIENT_SECRET` - The Port organization's client secret.

