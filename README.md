# mycool-kube-deploy
Kubernetes Test

Thank you Team for the assignment and the opportunity . Raising PR for your review . Things to review


1) Nodeapp directory for docker related
2) KubeAPP Directory
3) hpa Directory

PS : Also will send/attach a Microsoft Word file in the email along with this PR review


# Setup

1) Created a 2 node EKS Cluster in my AWS Account using resource Cluster config
2) Created a nodeapp directory hosting the contents for our backend nodejs application
3) Created directory  hpa for frontend php-apache demonstrating the example use case for HPA and liveness and readinessProbe
3) Kube-APP directory having all the templates/yaml files for NodeJS deployment.
4) Kube-APP directory also carries a mysql.yaml DB ( fake ) for assignment purposes ( for sharing secrets and persistentvolume demo)
5) Kube-APP also containts a secrets yaml file for  Secret resource deployment ( used in mysql)
6) a separate directory for hpa for handling frontend demonstrating the example use case for HPA and liveness and readinessProbe

PS : Will share a file separately on email showing screenshot of successful tests from my command line :)

# Scenario one - high Scalable and Available

 It should be highly available and scalable: The application has to be fault-tolerant
regarding the loss of one cluster node and automatically scalable depending on the CPU
load available across multiple nodes.


1)Created EKS cluster by cluster config as available in Kube-APP , Alternatively can run eksctl command(also mentioned in yaml file)
2)Enabled and configured EKS autoscaler for handling traffic in case if primary nodes in the autoscaling group fail to serve the traffic ( for this you need to run cluster-autoscale.yaml )
I have used https://docs.aws.amazon.com/eks/latest/userguide/cluster-autoscaler.html
3) Also tested the EKS Autoscaler by scaling replicas of frontend app php-apache (in hpa/deployment.yaml) from 2 to 20 by command
kubectl scale --replicas=20 deployment/php-apache , this will force cluster autoscaler to kick in and serve the traffic
4) Also if we increase the load on the service itself it will scale horizontally and add more pods in the deployments , also tested for php-apace by generating load on the service with load generator

## Give an example of how you would expose secure credentials as an environment variable. It could be faking the database connection even if the app doesn't need one.

1) created a sample Mysql deployment( stateful) using a combination of username and password from Secrets deployment
2) Its just a fake database for demonstrating having no contact with the php front end or nodejs but the idea is to highlight the
   use of PersistentVolume and PersistentVolumeClaim along with How to use Kubernetes Secrets .

##If the application is stateful - then it should have a persistent volume configuration.

1) Covered above , created a local mysqlvol in KubeAPP directory and created a persistentvolume and persistentVolumeClaim
2) Mounted in the sql deployment and created successfully . In this way we can mount the host volumes locally from EC2 etc to the containers

###You should make use of the readiness and liveness probe as much as feasible.

1) Introduced  Readiness and liveness probe in Frontend Php Apache just to make sure if application is working properly on the container port
2) Also we can Introduced Probes for lot of different use cases and can discuss further like for example some legacy, we can add certain wait conditions before by readiness probe etc
PS : Important thing is that must be configure properly and should not be a reason for application/container downtime somehow by misconfiguration
See hpa/deployment.yaml for this

###If possible, Ingress should be deployed for the opportunity to access the application outside of the private network

1)Since the application ( combination of frondend and backend) is only for demonstrating the different use cases of assignment and features of Kubernetes , I havent concentrated much on integrating both the php-apache and nodejs application and creating a ingress to make it publicly available . both these deployments are working locally
2) However a sample alb-Ingress.yaml is created which can be used to add more backends , but for now havent configured ingress

PS : Also keeping timelimit in mind , already investigated 6 plus hours on the assignment
Indeed was fun and learning :)
