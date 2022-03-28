# Deploy app in k8s


1. Start cluster

		$ minikube start

2. List services

		$ kubectl get all

3. List pods

		$ kubectl get pods

4. Deploy images

		$ kubectl create deployment redis --image=redis

		$ kubectl create deployment hasher --image=dockercoins/hasher:v0.1

		$ kubectl create deployment rng --image=dockercoins/rng:v0.1

		$ kubectl create deployment webui --image=dockercoins/webui:v0.1

		$ kubectl create deployment worker --image=dockercoins/worker:v0.1


5. Get pods

		$ kubectl get pods

		$ kubectl get podes -w

6. Show logs

		$ kubectl logs deploy/rng

		$ kubectl logs deploy/worker

7. Expose port

		$ kubectl expose deployment redis --port 6379

		$ kubectl expose deployment rng --port 80

		$ kubectl expose deployment hasher --port 80

		$ kubectl expose deploy/webui --type=NodePort --port=80

8. Show logs again

		$ kubectl logs deploy/worker

9. Get all services

		$ kubectl get service


## Kubectl-proxy

Expose internal and external services

1. Show IP services

		$ kubectl get services kubernetes

2. Run kuberctl-proxy

		$ kubectl proxy &

3. To access an API

		$ curl 127.0.0.1:8001

		$ curl http://localhost:8001/version

4. To access a port

		$ kubectl port-forward svc/redis 10000:6379 &

		$ telnet localhost 10000

## Dashboard

1. Run dashboard

		$ kubectl apply -f k8s/kubernetes-dashboard.yaml

		$ kubectl get services

2. To access dashboard

		$ kubectl edit service kubernetes-dashboard -n kube-system

3. Chage port

		```
		selector:
			k8s-app: kubernetes-dashboard
		sessionAffinity: None
		type: NodePort
		```

4. Access the dashboard through the browser

		$ minikube ip

		$ kubectl get service -n kube-system

## Load balancing and Daemon sets

1. Export rng settings

		$ kubectl get deploy/rng -o yaml > rng.yml

2. Change kind: `Deployment` to `DaemonSet`

3. Apply change

		$ kubectl apply -f rng.yml

		$ kubectl apply -f rng.yml --validate=false

		$ kubectl describe service rng

4. Add label

		$ kubectl get pods --selector=app=rng

5. Delete label

		$ kubectl label pod podname app-

## Manage deployment

1. Show deployments

		$ kubectl get deploy -o json

2. Update image

		$ kubectl set image deploy <name> worker=<image>:<tag>

3. Show deploys

		$ kubectl get replicasets -w

4. Update deploy at runtime

		$ kubectl edit deploy <name>

5. Rollback

		$ kubectl rollout undo deploy <name>
