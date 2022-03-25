# tekton-argocd-openshift-example
Example with Tekton + ArgoCD + OpenShift

# Enviroment

OpenShift 4.9   
OpenShift Pipelines 1.6.2   
OpenShift GitOps 1.4.3   

# First Run

~~~
curl -sSL https://raw.githubusercontent.com/brunobenchimol/tekton-argocd-openshift-example/main/install/01-argocd-app.yaml | oc apply -f -
sleep 30
curl -sSL https://raw.githubusercontent.com/brunobenchimol/tekton-argocd-openshift-example/main/install/99-ci-pipeline-run.yaml | oc -n php-http-vars create -f -
~~~

# Notes

1. Added namespace.yaml to work with replacements and create namespace, because argocd auto-create does not work on OpenShift.  
2. BuildConfig does not work properly because every namespace creates a different push/pull secret object.  

# Issues

1. ArgoCD has a hard time to start pipeline on first sync. Hooks runs on every sync. Because of that first pipeline run must be done manually.  

# TODO

1. Change TARGET namespace for project installation without changing YAML files.   
2. Improve installation for 1-click (either on ArgoCD or Script)   

# References

1. https://github.com/redhat-developer/openshift-gitops-getting-started
2. https://docs.openshift.com/container-platform/4.9/cicd/gitops/deploying-a-spring-boot-application-with-argo-cd.html
3. https://github.com/tektoncd/pipeline/issues/3202  
4. https://www.linkedin.com/pulse/continuous-delivery-helm-argo-cd-stephen-kuntz/