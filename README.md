# Port and Upbound demo
This is a guide for preparing a demo environment to showcase Port + Upbound capabilities, both as a software catalog and self-service hub for developers, and a platform management tool for the platform team.

The final goal of this demo is to interact with Upbound using Port, and provide EKSaaS using Upbound's capabilities and control planes as a backend, while reporting data regarding deployed clusters back to Port.

All needed manifests and information is in this git repository.

# In this repository
## <ins>Upbound XR template</ins>
In the `.upbound/examples/cluster.yaml` file ([link](https://github.com/port-demo/port-upbound-demo/tree/main/.up/examples/cluster.yaml)) is the default example provided by the EKSaaS configuration. This file is  used as a base template for creating new XRs in the Upbound control planes.

## <ins>Port assets</ins>
In this repository, there is a `.port/` directory ([link](https://github.com/port-demo/port-upbound-demo/tree/main/.port)). It holds manifests which will be used in this guide to create blueprints, and their corresponding Port actions.

## <ins>Github workflows - The Port actions backend</ins>
The `.github/workflows/` directory ([link](https://github.com/port-demo/port-upbound-demo/tree/main/.github/workflows)) holds the necessary Github action definitions. These actions will be used as the backend for the different Port actions for interacting with Upbound.

The demo starts off on a completely clean slate - an empty Upbound organization, an empty git repository, and a clean Port environment.

# Prerequisites
## <ins>Upbound</ins>
Before following the guide, you will need to set up an Upbound organization, initialize it and keep track of some information:
- Save the `Organization ID` for later;
- Set up the default EKSaaS configuration in the Upbound organization;
- Deploy a control plane (or many) and save their `identifiers` for later;
- Create an API token and save it for later.

## <ins>Port</ins>
It would be best to start off with a clean Port environment. Make sure that the Port organization used in the demo doesn't have any entities or blueprints.
Save the Port organization's `CLIENT_ID` and `CLIENT_SECRET` for later ([how to find your Port credentials](https://docs.getport.io/build-your-software-catalog/sync-data-to-catalog/api/#find-your-port-credentials)).

## <ins>Git repository</ins>
The actions backend, and the state of the different control planes will be handled in a github repository. For Port to interact with the new Github repo, you will need Port's Github app to be installed.

Create a new git repository, and make sure that Port's Github app is installed on it either by:
- installing Port's github app on all the repositories in the used Github organization;
- or by adding the new repository to the allowed repository list of Port's Github app in the organization.


# Demo guide
After completing the [prerequisites](https://github.com/port-demo/port-upbound-demo/blob/main/README.md#prerequisites) step, you can start following the guide.

## <ins>Setting up the git repository</ins>
### Create all the necessary files and directory structure
- `.github/workflows/` - Copy all of the `.yaml` files from this repository, to the new one in the same directory hierarchy. 

    Please note that in `delete-cluster.yaml` and `apply-clusters.yaml`, there is an env var configured:
    ```bash
        env:
            UPBOUND_ORG_ID: port # To be changed
    ```
    The value for this environment variable needs to match the Upbound `Organization ID` we set aside earlier.

- `.up/clusters/` - Create this folder in the repository. It will hold subdirectories which will correspond to different Upbound control planes. Each subdirectory will hold all the XR manifests to be deployed in the specific control plane.

- `.up/examples/cluster.yaml` - Copy this file to the new repository in the same directory hierarchy.

### Create repository secrets for the Github actions to use
Follow Github's [guide](https://docs.github.com/en/actions/security-guides/using-secrets-in-github-actions#creating-secrets-for-a-repository) to add required secrets to the repository. These are the secrets the need to be created:
* `UPBOUND_TOKEN` - The Upbound organization's API token;
* `PORT_CLIENT_ID` - The Port organization's client id;
* `PORT_CLIENT_SECRET` - The Port organization's client secret.

## <ins>Setting up Port</ins>
Starting with a clean Port organization, you will have to create some Port components. These components are Port [blueprints](https://docs.getport.io/build-your-software-catalog/define-your-data-model/setup-blueprint/#what-is-a-blueprint) and Port [actions](https://docs.getport.io/create-self-service-experiences/).

### Creating blueprints
In the `./port` folder are 3 `-BP.json` files numbered 1-3. These are JSON definitions for Port blueprints.

You will need to create these blueprints in your Port organization. 

__Create the blueprints in the order they are numbered__

Follow these steps:
1. Create a new custom blueprint from the Builder page;
![newCustomBlueprint](https://github.com/port-demo/port-upbound-demo/blob/main/static/newCustomBlueprint.png?raw=true)

2. Click the `Edit JSON` button;
![createBlueprintJson](https://github.com/port-demo/port-upbound-demo/blob/main/static/createBlueprintJson.png?raw=true)

3. Paste the blueprint `.json` contents in the editor and click `Save`.
![saveNewBlueprint](https://github.com/port-demo/port-upbound-demo/blob/main/static/saveNewBlueprint.png?raw=true)

*If a blueprint creation fails, make sure it was created in the correct order*

### Creating actions
In the `.port` folder are 2 `.json` files which define the different actions we need, to trigger the different Github workflows.

You will need to create these action definitions on their appropriate blueprints (notice the filenames `eksCluster-Actions.json` and `eksClusterRequest-Actions.json`).

_Please note that in each `-Actions.json` file, there are `CHANGE_TO_YOUR_GITHUB_ORG_NAME` and `CHANGE_TO_YOUR_REPO_NAME` values.  You need to replace these with your appropriate Github organization name and repository name._

To create the actions, follow these steps:

1. Expand the blueprint you wish to create an action for;
![expandBlueprint](https://github.com/port-demo/port-upbound-demo/blob/main/static/expandBlueprint.png?raw=true)

2. Edit click the `...` to open the blueprint's dropdown menu, and click `Edit Json`;
![editBlueprintAsJson](https://github.com/port-demo/port-upbound-demo/blob/main/static/editBlueprintAsJson.png?raw=true)

3. Switch to the `Actions` tab, and paste the contents of the `-Actions.json` in the editor and click `Save`.
![saveBlueprintActions](https://github.com/port-demo/port-upbound-demo/blob/main/static/saveBlueprintActions.png?raw=true)


### Creating Upbound control plane Port entities
After setting up the Port blueprints and actions, we need to insert some entities manually.

These entities will represent the different Upbound control planes which were created earlier.

To do this, follow these steps:

1. Navigate to the [Upbound control planes](https://app.getport.io/upbound_control_planes) catalog page.

2. Click the `Manually add Upbound Control Plane` button (or the `+ Upbound Control plane` at the top right of the page).
![addUpboundControlPlaneManually](https://github.com/port-demo/port-upbound-demo/blob/main/static/addUpboundControlPlaneManually.png?raw=true)

3. In the `Title` field, insert the Upbound control plane `identifier` which we saved earlier and click create (do this step multiple times if there are more than 1 control planes).
![setUpboundControlPlaneIdentifier](https://github.com/port-demo/port-upbound-demo/blob/main/static/setUpboundControlPlaneIdentifier.png?raw=true)

# Using Port
At this point, everything should be set up. Browse to your [Self-service](https://app.getport.io/self-serve) page to view the different actions you defined in Port, and try them out.

![selfServicePage](https://github.com/port-demo/port-upbound-demo/blob/main/static/selfServicePage.png?raw=true)




