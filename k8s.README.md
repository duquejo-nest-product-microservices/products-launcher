
## K8s configuration

### Installation
1. Install K8s Helm (Kubernetes package manager)
   ```shell
   choco install kubernetes-helm
   ```

---

# Helm commands

* Create configuration: `helm create <name>`
* Apply initial configuration: `helm install <name> .`
* Apply updates: `helm upgrade <name> .`

# K8s commands

* Get pods, deployments, and services: `kubectl get <pods | deployments | services>`
* Check all pods: `kubectl describe pods`
* Check a specific pod: `kubectl describe pod <name>`
* Delete a pod: `kubectl delete pod <name>`
* Check logs: `kubectl logs <name>`

# Create a deployment:
```
kubectl create deployment <name> --image=<registry/url/image> --dry-run=client -o yaml > deployment.yml
```

# Create a service
```
kubectl create service clusterip <name> --tcp=<8888> --dry-run=client -o yaml > service.yml 
kubectl create service nodeport <name> --tcp=<3000> --dry-run=client -o yaml > service.yml
```
* **clusterip**: accessible only from within the cluster
* **nodeport**: accessible from outside the cluster

# Secrets

* Create secrets, multiple at once or one by one.
```
kubectl create secret generic <name> --from-literal=key=value

kubectl create secret generic secret1 --from-literal=key1=value1 --from-literal=key2=value2
```
* Get the secrets: `kubectl get secrets`
* View a secret's contents: `kubectl get secrets <name> -o yaml`

## Edit a secret (using VIM)
The easiest way is to delete and recreate it, but if there are multiple secrets, you won’t want to lose the others. Remember that secrets are in `base64`, so if you want to edit a secret, it must be done in `base64`.

1. Edit the secret with `kubectl edit secret <name>`, which will open the editor.
2. Change the value (you can use an [online editor to convert to base64](https://www.rapidtables.com/web/tools/base64-decode.html)).
3. Press **i** to insert lines and edit the file.
4. Place the value to decode on a new line.
5. Press **esc** and then `:. ! base64 -D` to decode the value.
6. Press **i** to insert or edit the value.
7. Press **esc** and then `:. ! base64` to encode the value.
8. Edit the file again with **i** and leave the line in position.
9. Press **esc** and then **:wq** to save and exit.

## Configure Google Cloud secrets to pull images

1. Create the secret:
```
kubectl create secret docker-registry gcr-json-key --docker-server=GOOGLE-SERVER-docker.pkg.dev --docker-username=_json_key --docker-password="$(cat 'PATH/TO/Google IAM json')" --docker-email=YOUR_EMAIL@gmail.com
```

2. Add the secret path to use the key:
```
kubectl patch serviceaccounts default -p '{ "imagePullSecrets": [{ "name":"gcr-json-key" }] }'
```

## Export and apply configurations using files (secrets in this case)
* To export configuration files:

```
kubectl get secret <name> -o yaml > <name>.yml
```

* Apply configuration based on the file:
```
kubectl create -f <name>.yml
```


###
Changing contexts:

Visualizing available contexts:
```bash
kubectl config get-contexts
```

Changing current context to local (docker-desktop)
```bash
kubectl config use-context docker-desktop
```