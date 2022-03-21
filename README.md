# tekton-argocd-openshift-example
Example with Tekton + ArgoCD + OpenShift

# Enviroment

OpenShift 4.9   
OpenShift Pipelines 1.6.2   
OpenShift GitOps 1.4.3   

# Pre-Reqs

You must create namespace first and label it for ArgoCD be able to manage it.   
`oc create namespace <name>`   
`oc label namespace <name> argocd.argoproj.io/managed-by=openshift-gitops`   

# References

1. https://github.com/redhat-developer/openshift-gitops-getting-started
2. https://docs.openshift.com/container-platform/4.9/cicd/gitops/deploying-a-spring-boot-application-with-argo-cd.html