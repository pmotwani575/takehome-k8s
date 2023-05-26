## Take Home SRE Test

Kubernetes Test

Thank you Team for the assignment and the opportunity . Please read the main branch which contains the code . Please read below

1) Nodeapp directory for docker related stuff
2) KubeAPP Directory showing cluster and backend manifests files
3) FrondEnd Directory having example showing FE deployment , service, HPA and probes

PS : Also will attach a Microsoft Word file in the email showing the successfully deployment in test environment locally

## Setup

1) Created a 2 node EKS Cluster in my AWS Account using resource Cluster config
2) Created a nodeapp directory hosting the contents for our backend nodejs application
3) Created directory FrondEnd for frontend php-apache demonstrating the example use case for HPA and liveness and readinessProbe
3) Kube-APP directory having all the templates/yaml files for NodeJS deployment.
4) Kube-APP directory also carries a mysql.yaml DB ( fake ) for assignment purposes ( for sharing secrets and persistentvolume demo)
5) Kube-APP also containts a secrets yaml file for  Secret resource deployment ( used in mysql)

## Scenario one - high Scalable and Available

 It should be highly available and scalable: The application has to be fault-tolerant
regarding the loss of one cluster node and automatically scalable depending on the CPU
load available across multiple nodes.

1)Created EKS cluster by cluster config as available in Kube-APP , alternatively we can create cluster by using Terraform(EKS Blueprints), EKSCTL , manually or by any other tool.

2)Enabled and configured EKS autoscaler for handling traffic in case if primary nodes in the autoscaling group fail to serve the traffic ( for this you need to run cluster-autoscale.yaml )
I have used https://docs.aws.amazon.com/eks/latest/userguide/cluster-autoscaler.html
Alternatively we can also use Karpenter or any other open source for cluster node management

3) Also tested the EKS Autoscaler by scaling replicas of frontend app php-apache (in hpa/deployment.yaml) from 2 to 20 by command
kubectl scale --replicas=20 deployment/php-apache , this will force cluster autoscaler to kick in and serve the traffic

4) Also if we increase the load on the service itself it will scale horizontally and add more pods in the deployments , also tested for php-Apache by generating load on the service with load generator

## Give an example of how you would expose secure credentials as an environment variable. It could be faking the database connection even if the app doesn't need one.

1) created a sample Mysql deployment( stateful) using a combination of username and password from Secrets deployment

2) Its just a fake database for demonstrating having no contact with the php front end or nodejs but the idea is to highlight the
   use of PersistentVolume and PersistentVolumeClaim along with How to use Kubernetes Secrets .

Note : Not the best way to store secrets , in Live environments we can use external or cloud secret services like AWS Secret manager etc which are more secure

## If the application is stateful - then it should have a persistent volume configuration.

1) As mentioned above , Created a local mysqlvol in KubeAPP directory and created a persistentvolume and persistentVolumeClaim

2) Mounted in the sql deployment and created successfully . In this way we can mount the host volumes locally from EC2 etc to the containers

## You should make use of the readiness and liveness probe as much as feasible

1) Introduced  Readiness and liveness probe in Frontend Php Apache just to make sure if application is working properly on the container port number

2) Also we can Introduced Probes for lot of different use cases and can discuss further like for example - For some legacy or slow moving applications , we can add certain wait conditions before by readiness probe etc or we can add probes in case if there is a requirement for INIT containers , so can be different probes depending upon the requirements

PS : Important thing is that must be configure properly and should not be a reason for application/container downtime somehow by misconfiguration

## If possible, Ingress should be deployed for the opportunity to access the application outside of the private network

1) Keeping time limits in mind and other Objects which were must for the assignment ( such as HPA , cluster scaler , probes etc) , I have not configured Ingess object or controller for this test but certainly we can discuss during interview

2) However a sample alb-Ingress.yaml is created for review , added a annotation of ALB type to work with AWS ALB Controller , additionally we can use Nginx or any other controller/Ingress type depending upon the use case or decision.

## Things I would like to Improve in my assignment if I have more Time or if this is Live environment :)

1) For Cluster creation we can use tools like Terraform  or EKS Blueprints projects if we are using AWS , moreover we can improve the overall deployment by adapting Gitops approach by using tools like Argo or Flux .

2)For creating deployments or K8s Objects , can adapt tools like Helm or Kustomize which will ensure we are packaging out end to end application and reduce repetition and provide more control and security.

3) For Cluster Node Scaling , Autoscaler is nice but we can go for 3rd party tools like Spot IO which will reduce the management overhead or Karpenter or may be we can also build our infrastructure on GCP with GKS service which provides more open source and k8s native flexibility

4) For secrets I am using K8s native Secret resource but we can use external secrets provider services , like AWS Secrets manager ,  or by integrating k8s with something like Hashicorp Vault .
Integrate CRDs or external controllers for secrets like Godaddy Secrets manager which can be integrated with AWS Secrets manager

5) Ingress objects and controller for external communication

6) Can Apadt Service mesh for better security with mutual tls or better visibility
