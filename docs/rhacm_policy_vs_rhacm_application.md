# Policy versus Application on RHACM to deploy application

RHACM provides several mechanism to deploy applications on the managed clusters. 

The more natural is the use of RHACM applications. This feature is based on GitOps approach. It allows to deploy an application that has its definiton into a Git Application (RHACM support other channels like helm or Object storage but here I'm focusing on Git channel). The definition could contains CR related to an operator, config maps secret amd more. 

The second one that could be considered is the Config policy. It checks if a resources (cr, secret or other resource) is present. If not, if the policy is configured to enforce, it could create the resource on the managed cluster. 

Both approach could answer the question of application deployment

But I see two main differences on the apporach. 

The first one is on agility. The poliy should contains the definiton of the object inside its defintion. [here](../rhacm/config-policies/mq-operator.yaml) is a exemple of the subscription used to deploy MQ operator. The subscription is totaly part of the policy in the `objectDefitnion` parameter. With the Application approach, the resource to deploy are store into git directory independly of the application obkect used to deploy them. It provides a better agility as the Deployment CRD could be defined by developers without any knowledge of RHACM. The policy defintion need RHACM skills.

The second topic is about the deletation of the resource and its lifecycle. With  the policy approach, the object is created if the policy detect its missing. But if you delete the policies, the enforced resources are not deleted on managed cluster. You must go on each managed cluster where the resource has been enforced (accordingly to placement rules and bindding) and remove the resource. With the application approach, when the application is deleted, the  deployed resources are deleted as well. The two approaches provides two different lifecycle. 

Regarding the placement of managed clusters, both approaches work with placement rules.

I think the two appraoches should be not opposed. Each provides benefits. Both could be used together on an deployment strategy. The policy approach should be used to ensure the mandatory compoments are present on managed clusters. And as application could have a different lifecycle than the cluster, they should leverage RHACM application to intiate their deployement. 

On this demonstration, the two approaches are used. Bootstrap configuration of the managed cluster is done via configuration policies. It includes the deployement of RH GitOps, the defintion of IBM operators catalogs and needed operators. I also used the configuration policy to provide the ibm entilement key to my projects. Applications (CPFS and MQ) are deployed via RHACM application