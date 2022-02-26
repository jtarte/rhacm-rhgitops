# Manage the entitlement or registry key

Depending if you are using the IBM entitled registry (cp.icr.io) or a private one (in the case of an Argap installation), you have to manage the docker credentials allowing the OCP cluster to pull the CP4I images. 

If you are not willing to define this information at cluster level (global pull secret), you have to provide the information inside each project where the CP4I components are deploy. 

To automate this, I used a RHACM Config policy. It checks if an secret with `kubernetes.io/dockerconfigjson` type is define in the project. It not, as remediation, RHACM will enforce the creation of this secret.

This policy is apply on some selected namespace, those that are compliants with `cp4i-*`pattern in their name. 

The policy could be find [here](../rhacm/config-policies/entitlement-key.yaml).

```
apiVersion: policy.open-cluster-management.io/v1
kind: Policy
metadata:
  name: entitlement-key-policy
spec:
  remediationAction: enforce
  disabled: false
  policy-templates:
    - objectDefinition:
        apiVersion: policy.open-cluster-management.io/v1
        kind: ConfigurationPolicy
        metadata:
          name: entitlment-key
        spec:
          remediationAction: inform
          severity: low
          namespaceSelector:
            exclude:
              - kube-*
            include:
              - cp4i-*
          object-templates:
            - complianceType: musthave
              objectDefinition:
                kind: Secret
                apiVersion: v1
                metadata:
                   name: ibm-entitlement-key
                data:
                  .dockerconfigjson: >-           
                     < put here your base64 encoded docker login information>
                type: kubernetes.io/dockerconfigjson
```

Before to used this policy, you must file the `.dockercnfigjson` with your docker login information (registry, user, password ). The expected string is the base64 encoded form of this credentials. 