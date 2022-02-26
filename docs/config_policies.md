# Use to Config policyes to bootstrap the configuration of managed cluster

On this demonstration, RHACM Config policies are used to bootstrap the configuration of managed cluster. It will deploy the layer of mandatory conpoments (operators,...) and mandatory configurations.

The Config policies used in the demonstration is:

* [rhgitops](../rhacm/config-policies/rhgitops.yaml). It deploys the RH GitOps operator and the gitops cluster(ArgoCD) . It defines also a `clusterrole` and a `clusterrolebinding` to grant additonal priviledges to argocd service account. This addtional priviledges are expected by the dmoentratipn (creation of a new project on the cluster).
* [ibm-catalog](../rhacm/config-policies/ibm-catalog.yaml).It loads the IBM Operators catalog on the targeted cluster.
* [mq-operator](../rhacm/config-policies/mq-operator.yaml). It deploys the IBM MQ operator on the targeted cluster.
* [apic-operator](../rhacm/config-policies/apic-operator.yaml). It deploys the IBM APIC operator on the targeted custer.
* [cpfs-operator](../rhacm/config-policies/cpfs-operator.yaml). It deploys the IBM Cloud Pak foundational services operator on the targeted custer.
* [entitlement-key](../rhacm/config-policies/entitlement-key.yaml). It defines a rule to enforce the creation of a secret on project that will use CP4I components. More details [here](./entitlement.md)

The policies are applied on managed cluster accordingly with `PlacementRule` and `PlacementBinding`.  
