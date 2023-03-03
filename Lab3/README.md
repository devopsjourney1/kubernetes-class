
# Lab 3: Deploying a Web Application Using Helm


## Setup
- Do a `minikube delete` and `minikube start` before attempting this lab. If you don't do this then you will run into issues that some of the resources already exist.
- Manually create namespaces:
```
kubectl create namespace dev
kubectl create namespace prod
```


## Create the Scaffolding of the Helm Chart
Create the Scaffolding of a Helm Chart. In the real world - this step is optional. You could create a Helm Chart just by copying/pasting the files from another Helm Chart. Copy/Pasing another Helm Chart is what most people do.

```
helm create myhelmapp
```

## Inspect the folder struct and remove unnecessary files

- A new folder named mywebapp should of been created
- Look at the files it created. Specifically `Chart.yaml` and `values.yaml`
- Now look at all the files in the myhelmapp/templates folder. These are just there for examples
- Remove all the files in the myhelmapp/templates folder. We want to use our own manifest files


## Move our manifest files into the templates folder

- Take the manifests we have from Lab2 and move them into the templates folder that was created
- remove the `namespaces.yml` file. 

## Modify the templates to use Helm templating

Look at the `deployment.yml`, `services.yml` and `configmap` files. What are some things we can customize?

The answer - Anything we want. Customize the following:

`{{ .Values.namespace }}` for the namespace
`{{ .Values.appName }}` for all the app: labels
`{{ .Values.image.name }}` for the image name
`{{ .Values.image.tag }}` for the image tag
`{{ .Values.configmap.name }}` for the config map name

Note: you will probably want to check the solutions folder, there are a lot of changes to make.

## Modify the values.yaml file

- The helm create command would of created a values.yaml file. Inpsect it.
- Again, this is just an example. We can customize anything we want. Remove the contents of the file and replace it with the following:

```
appName: myhelmapp

namespace: default

configmap:
  name: nginx-configv1

image:
  name: nginx
  tag: latest
```

## Deploy your chart using helm

```
helm install myhelmapp myhelmapp/. 
```


## Verify the deployment


```
helm ls
kubectl get all
```

## Create a prod and dev version

- copy/paste your values.yml file to create two more: values-dev.yml and values-prod.yml
- change some of the values. namespace, image tag, configmap name, replicas
- deploy your prod and dev versions

```
helm install myhelmapp-dev myhelmapp/. --values myhelmapp/values.yaml --values myhelmapp/values-dev.yaml
helm install myhelmapp-prod myhelmapp/. --values myhelmapp/values.yaml --values myhelmapp/values-prod.yaml
```


## Verify the deployments

```
helm ls
kubectl get all -n <namespace>
```

## Troubleshooting

Mess up a deployment? you may have to `helm uninstall`

example:

```
helm uninstall myhelmapp-dev
```