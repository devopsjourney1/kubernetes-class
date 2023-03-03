
# Apply the configus in the folder. you can use the command below

1. Apply the namespace first
```
kubectl apply -f ./Lab2/namespaces.yml
```
2. Now you should be able to apply all the resources

```
kubectl apply -f ./Lab2/
```

# Get access to your service through Minikube

In a new terminal window, run this command and leave it running.  It will provides you with a url to access your application.  You can use the url to test your application.

```
minikube service -n dev --all
```

# Get the pods in the dev namespace and watch for changes

Note: YOu can try adding the -w flag to watch for changes! Use Ctrl+C to exit
```
kubectl get pods -n dev
```

# Make changes to your Deployment 

1. Change it from configmapv1 to configmapv2 (line 40)
2. Reapply your manifests
3. Look at your pods and notice they should of been recreated
4. Use the url provided.  You should see your application is now using configmapv2

```
kubectl apply -f ./Lab2/
kubectl get pods -n dev
```

# Roll back your Deployment to configmap1!

1. Change it from configmapv2 to configmapv1 (line 40)
2. Reapply your manifests
3. Look at your pods and notice they should of been recreated
4. Use the url provided.  You should see your application is now using configmapv1

```
kubectl apply -f ./Lab2/
kubectl get pods -n dev
```