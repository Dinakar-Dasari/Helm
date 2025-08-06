## Helm:
   + Helm is a package manager for Kubernetes, often described as the "apt" or "npm" for Kubernetes. It simplifies the deployment, management, and scaling of applications on Kubernetes clusters by bundling all necessary resources into reusable, versioned packages called charts.
   + **_Kubernetes:_**
     + It is a system for orchestrating containerized applications, managing things like deployment, scaling, and updates across multiple servers.
   + **_Helm charts:_**
     + Those are pre-configured templates that define Kubernetes resources (e.g., pods, services, config maps) in a structured, reusable way.
     + A Helm chart is a collection of YAML files that define a Kubernetes application — including templates, default values, and metadata.
   + _Think of Helm as a tool that streamlines the process of installing and managing complex applications on Kubernetes_

**_Key Concepts of Helm:_**
   + **_Charts:_** A Helm chart is a collection of YAML files that describe a set of Kubernetes resources (e.g., deployments, services, secrets). It’s like a blueprint for deploying an application.
   + **_release:_**
      + When you deploy a chart to a Kubernetes cluster, Helm creates a release—a specific instance of that chart running in your cluster. You can have multiple releases of the same chart with different configurations.
      + A Helm release is a running instance of a Helm chart deployed into a Kubernetes cluster, along with its configuration and revision history.
      
**_Folder structure:_**
```
  my-chart/
  ├── charts/              --------> # Directory for dependency charts (optional)
  ├── templates/           --------> # Directory for Kubernetes resource templates 
  │   ├── deployment.yaml  --------> # Template for Kubernetes Deployment
  │   ├── service.yaml     --------> # Template for Kubernetes Service 
  │   └── ...
  ├── values.yaml          --------> # Defines default configuration values for your chart.
  ├── Chart.yaml           --------> # Metadata about the chart (name, version, description, etc.) 
  └── README.md            --------> # Documentation for the chart (optional)  
```
 

**Commands:** 
   + `helm install nginx . --> ". represents current directory"`: Deploys your application for the first time
     + What does helm install do under the hood?
       + Reads all files in templates/
       + Loads the variables from values.yaml (and overrides)
       + Renders final YAMLs by replacing **placeholders** (will replace this placeholders with values.yml file)
       + Sends these YAMLs to Kubernetes using kubectl apply internally
       + Tracks this deployment as a release (with versioning)
      
   +  `helm upgrade nginx .:` If you already ran helm install, you don’t need to install again. Use helm upgrade to update the existing release.
       + When you Change the chart, Change the values file, Change configs
    
   +  `helm rollback:` Goes back to a previous version of the release. You did helm upgrade and your app broke → use helm rollback to revert.
    
**Why Do We Need Helm?**  
   + Kubernetes is powerful but complex. Deploying an application involves creating and managing multiple YAML files for resources like pods, services, and ingress rules. Doing this manually is error-prone, repetitive, and hard to scale
   + Helm addresses these challenges by:
     + **Simplifying Deployment:** Instead of writing and managing dozens of YAML files, you use a single Helm chart to deploy an entire application
     + **Reusability and Consistency:** Charts are reusable templates. You can deploy the same chart (e.g., Nginx) across different clusters or environments (dev, staging, production) with customized settings.
     + **Versioning and Rollbacks:** Helm tracks releases, allowing you to version your deployments. If an update fails, you can roll back to a previous version with helm rollback.

**When Should You Use Helm?**  
   + You’re managing complex applications: Helm shines with apps requiring multiple Kubernetes resources.
   + You need consistency across environments: Helm ensures dev, staging, and prod deployments are identical except for configured differences.

 + What is the purpose of values.yaml?
   + values.yaml is a file where you define default configuration values (like image name, port, replica count, etc.).
   + These values are referenced inside templates using {{ .Values.<key> }}.
   + You can override these values at runtime using `-f or --set flags`
  
 + How can Helm help you change the image tag only for dev?
   + We create separate values files per environment — like:
   + dev-values.yaml
   + prod-values.yaml
   + run `helm install myapp ./mychart -f dev-values.yaml`

























