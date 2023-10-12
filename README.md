# Port and Upbound demo
This is a step-by-step guide for preparing a demo environment to showcase Port + Upbound capabilities, both as a software catalog and self-service hub for developers, and a platform management tool for the platform team.

The final goal of this demo is to interact with Upbound using Port, and provide EKSaaS using Upbound's capabilities and control planes as a backend, while reporting data regarding deployed clusters back to Port.

All needed manifests and information is in this git repository.

# In this repository
## Port assets
In this repository, there is a `.port/` directory ([link](https://github.com/port-demo/port-upbound-demo/tree/main/.port)). It holds manifests which will be used in this guide to create blueprints, and their corresponding Port actions.

## Github workflows - The Port actions backend
The `.github/workflows/` directory holds the necessary Github action definitions. These actions will be used as the backend for the different Port actions for interacting with Upbound.

# Demo setup guide
The demo starts off on a completely clean slate - an empty Upbound organization, an empty git repository, and a clean Port environment.

## Prerequisites
### Upbound
Before following the guide, you will need to set up an Upbound organization, initialize it and keep track of some information:
- Save the `Organization ID` for later;
- Set up the default EKSaaS configuration in the Upbound organization;
- Create an API token and save it for later.

### Port 
It would be best to start off with a clean Port environment. Make sure that the Port organization used in the demo doesn't have any entities or blueprints.
Save the Port organization's `CLIENT_ID` and `CLIENT_SECRET` for later.

### Git repository
The demo, and the state of the different control planes will be handled in a github repository. 
Create a new git repository, and make sure the Port's Github app is installed on it either by:
- installing Port's github app on all the repositories in the used Github organization;
- or by adding the new repository to the allowed repository list of Port's Github app in the organization.


