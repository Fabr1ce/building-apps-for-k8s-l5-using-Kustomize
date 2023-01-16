# building-apps-for-k8s-l5

This repo is used to practice k8s deployment and service manifests. Concepts covered here are also explained in the [k8s docs](https://kubernetes.io/docs/concepts/workloads/):

1. pods (groud of one or more containers with single IP)

2. replicaSet (way to deploy multiple identical pods at once)

3. deployment (manages replicasets plus rolling updates)

4. service (provides a stable network address for apps including local IP that allows pods to be destroy while the remaining pods have this IP where traffic is directed, DNS within the cluster, it load balances traffic accross pods)

5. kustomize manages apps without the need of templates.

## The following steps will deploy a k8s deployment and a service:

1. Create a cluster: refer to https://github.com/Fabr1ce/building-apps-for-k8s-l4 for steps on how to create a kubernetes cluster.

2. Write YAML files to pass to k8s or use the ones in this repo and use the following cmds:

### Create the deployment and verify:

`kubectl apply -f deployment.yaml`

`kubectl get pods`
	

## Create the service:

`kubectl apply -f service.yaml`

`kubectl get services` 
	

**From here:**
- service IP can be accessed through cURL like this exec <outside-pod-name> curl <cluster-ip>
- pods can be accessed through cURL like this exec <outside-pod-name> curl <pod-ip>:<port-in-manifest>. Notice the difference between Port (port service listens on and forwards to NodePort) and NodePort (port pod listens on)
- clean up:
	
`kubectl delete -f service.yaml`
	
`kubectl delete -f deployment.yaml`

**Using Kustomize:**
Kustomize takes all the yaml files and creates all the components/objects/environments using the following cmds:
	
`kustomize build base`

`kustomize build overlays/production`

`kustomize build overlays/production | kubectl apply -f -`
	
Kustomize here took the base resource manifest and patched them with overlays.
