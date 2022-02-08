---
id: yp9TieRffClTFFXhiUSmT
title: AzureML
desc: ''
updated: 1644292687574
created: 1644291480259
---

The [[langs.python]] SDK for [[azure.ml.sdk]]

## Load a Workspace
> See: [[courses.learn.work-with-azure-machine-learning-deploy-serving-models]]

```py
import azureml
from azureml.core import Workspace

workspace_name = "<workspace-name>"
workspace_location="<workspace-location>"
resource_group = "<resource-group>"
subscription_id = "<subscription-id>"

workspace = Workspace.create(name = workspace_name,
                             location = workspace_location,
                             resource_group = resource_group,
                             subscription_id = subscription_id,
                             exist_ok=True)
```

When running on [[azure.ml.studio]] it can grab workspace etc. from the subscription.

## Deploying a Model

### Build a Container Image for a Model
> See: [[courses.learn.work-with-azure-machine-learning-deploy-serving-models]]

```py
import mlflow.azureml

model_image, azure_model = mlflow.azureml.build_image(model_uri=model_uri, 
                                                      workspace=workspace,
                                                      model_name="model",
                                                      image_name="model",
                                                      description="Some description",
                                                      synchronous=False)
model_image.wait_for_creation(show_output=True)
```

### Create an ACI Deployment
> See: [[courses.learn.work-with-azure-machine-learning-deploy-serving-models]]

```py
from azureml.core.webservice import AciWebservice, Webservice

dev_webservice_name = "my-model"
dev_webservice_deployment_config = AciWebservice.deploy_configuration()

dev_webservice = Webservice.deploy_from_image(name=dev_webservice_name, image=model_image, deployment_config=dev_webservice_deployment_config, workspace=workspace)

dev_webservice.wait_for_deployment()
```

### Create an AKS Cluster
> See: [[courses.learn.work-with-azure-machine-learning-deploy-serving-models]]

```py
from azureml.core.compute import AksCompute, ComputeTarget

# Use the default configuration (you can also provide parameters to customize this)
prov_config = AksCompute.provisioning_configuration()

aks_cluster_name = "my-cluster" 

# Create the cluster
aks_target = ComputeTarget.create(workspace = workspace, 
                                  name = aks_cluster_name, 
                                  provisioning_configuration = prov_config)

# Wait for the create process to complete
aks_target.wait_for_completion(show_output = True)
print(aks_target.provisioning_state)
print(aks_target.provisioning_errors)
```

### Connect to an Existing AKS Instance
> See: [[courses.learn.work-with-azure-machine-learning-deploy-serving-models]]

```py
from azureml.core.compute import AksCompute, ComputeTarget

# Give the cluster a local name
aks_cluster_name = "my-cluster"

aks_target = ComputeTarget(workspace=workspace, name=aks_cluster_name)
print(aks_target.provisioning_state)
print(aks_target.provisioning_errors)
```

### Deploy an Image to AKS
> See: [[courses.learn.work-with-azure-machine-learning-deploy-serving-models]]

```py
from azureml.core.webservice import Webservice, AksWebservice

# Set configuration and service name
prod_webservice_name = "my-model-prod"
prod_webservice_deployment_config = AksWebservice.deploy_configuration()

# Deploy from image
prod_webservice = Webservice.deploy_from_image(workspace = workspace, 
                                               name = prod_webservice_name,
                                               image = model_image,
                                               deployment_config = prod_webservice_deployment_config,
                                               deployment_target = aks_target)

# Wait for the deployment to complete
prod_webservice.wait_for_deployment(show_output = True)
```