To be able to run the pods and services on Kubernetes
1. The region for deployment is set using `gcloud config set compute/region us-central1`
2. The cluster was created using `gcloud container clusters create yolo --num-nodes=1`

 ## yolo-deployment.yaml structure
 - The backend service is created as a Kubernetes Service resource, with a name of "backend". The spec field defines the Service's behavior. In this case, the selector field specifies that the Service should select all Pods with the label "app: backend". The ports field specifies that the Service should listen on port 5000 and forward traffic to port 5000 on the selected Pods. The type field specifies that the Service should be a ClusterIP, which means it will only be accessible within the cluster.

- The client service is also created as a Kubernetes Service resource, with a name of "client". The spec field is similar to the backend service's spec, with the selector field selecting Pods with the label "app: client", the ports field specifying that the Service should listen on port 3000 and forward traffic to port 3000 on the selected Pods, and the type field specifying that the Service should be a ClusterIP.

- The mongodb service is also created as a Kubernetes Service resource, with a name of "mongodb". The spec field is similar to the other services, with the selector field selecting Pods with the label "app: mongodb", the ports field specifying that the Service should listen on port 27017 and forward traffic to port 27017 on the selected Pods, and the type field specifying that the Service should be a ClusterIP.

- Two deployments are also created, one for the backend service and one for the client service. Each deployment has a metadata field specifying the deployment's name, and a spec field that defines the deployment's behavior. The replicas field specifies that only one replica of each pod should be created. The selector field specifies which Pods the deployment should manage. The template field specifies the Pod's template, including its labels and containers.

- The backend pod's template includes two containers, one for the backend service and one for the database service. The backend container runs the "molline/yolo-backend:1.0.0" image, listens on port 5000, and sets an environment variable "MONGODB_URI" to "mongodb://mongodb:27017/yolomy". It also specifies that the "npm run start" command should be run. The mongodb container runs the "mongo" image, listens on port 27017, and mounts a volume to "/data/db" to persist data.

- The client pod's template includes one container that runs the "molline/yolo-client:1.0.0" image, listens on port 3000, and sets an environment variable "MONGODB_URI" to "mongodb://mongodb:27017/yolomy". It also specifies that the "npm start" command should be run.

## yolo-service.yaml
- apiVersion: v1: This specifies the Kubernetes API version to use, which is v1 in this case.

- kind: Service: This indicates that we are defining a Kubernetes Service resource.

- metadata: This section provides metadata about the service, such as its name.

- name: yolo: This sets the name of the service to "yolo".

- spec: This section defines the service's specifications.

- ports: This specifies the port(s) that the service should listen on, as well as the target port(s) that traffic should be forwarded to.

    - port: 3000: This sets the service's listening port to 3000.

    - targetPort: 9376: This specifies that traffic should be forwarded to port 9376 on the pods selected by the service.

- selector: This specifies how the service should select the pods to forward traffic to.

    - app: client: This selects all pods with a label of "app: client".

- type: LoadBalancer: This specifies the type of the service. In this case, it is a LoadBalancer service, which allows external traffic to be routed to the selected pods.
## Application access point
The app will be accessible on `http://34.135.193.213:3000/`
