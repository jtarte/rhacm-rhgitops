# RHACM Application versus ArgodCD(RH gitops) application

Both could deploy an application. Both are using Gitops approach. 

But, in this demonstration, I use RHACM application only to bootstrap the ArgoCD application deployment. In this way, RHACM delegates the application deployment to ArgoCD(RH gitops).

So the question is why having this two layers approach. 

First, it keeps RHACM as the control tower, it initiiates the deployment of the diverse clusters.

The second point to consider is that ArgoCD offer more features to manage applicatin deployement, espacially with Waves and Hooks. 

NB: the architure used on this demonstration is decentralized. ArgoCd is deployed on managed cluster and drive the application deployement from the managed cluster.

Remarks: the use of both seems a direction that Red Hat is following. Integration of ArgoCd into RHACM is staritng. with the 2.4, you could find as tech preview the use of ArgoCD application sets. And deployment made by ArgoCd is automatically detected by RHACM.   