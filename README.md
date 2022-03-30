# php-http-vars-cicd
CI/CD Example with Tekton + ArgoCD + OpenShift

# Enviroment

OpenShift 4.9   
OpenShift Pipelines 1.6.2   
OpenShift GitOps 1.4.3   

# First Run

~~~
curl -sSL https://raw.githubusercontent.com/brunobenchimol/php-http-vars-cicd/main/install/01-argocd-app.yaml | oc apply -f -
sleep 30
curl -sSL https://raw.githubusercontent.com/brunobenchimol/php-http-vars-cicd/main/install/99-ci-pipeline-run.yaml | oc -n php-http-vars create -f -
~~~

# Triggering Pipeline

If you would like to test your CI you can either push to git repository or trigger manually using cURL. If you do remember to change to match your repository (including other files on git that contains git URL)  
~~~
 URL="https://github.com/brunobenchimol/php-http-vars" 
 ROUTE_HOST=$(oc get route el-php-http-vars --template='https://{{.spec.host}}')
 curl -k -v \
    -H 'X-GitHub-Event: push' \
    -H 'Content-Type: application/json' \
    -d '{
      "head_commit": {"id": "main", "message": "fake-push-with-cURL" },
      "repository": { "url": "'"${URL}"'", "name": "php-http-vars"},
      "pusher": {"name": "cURL"}
    }' \
    ${ROUTE_HOST}
~~~

# Notes

1. Added namespace.yaml to work with replacements and create namespace, because argocd auto-create does not work on OpenShift.  
2. BuildConfig does not work properly because every namespace creates a different push/pull secret object.  
3. OpenShift needs a `ImageStreamTag` object to be able to triggers deployment on image change.  

# Issues

1. ArgoCD has a hard time to start pipeline on first sync. Hooks runs on every sync. Because of that first pipeline run must be done manually.  
2. OpenShift always change image tag when using triggers and causes ArgoCD to go Out of Sync.   
`Fix`: "Ask" Tekton CI to "restart deployment" to pick new image.  

# TODO

1. Change TARGET namespace for project installation without changing YAML files.   
2. Improve installation for 1-click (either on ArgoCD or Script)   

# References

1. https://github.com/redhat-developer/openshift-gitops-getting-started
2. https://docs.openshift.com/container-platform/4.9/cicd/gitops/deploying-a-spring-boot-application-with-argo-cd.html
3. https://github.com/tektoncd/pipeline/issues/3202  
4. https://www.linkedin.com/pulse/continuous-delivery-helm-argo-cd-stephen-kuntz/
5. https://developer.ibm.com/tutorials/tekton-triggers-101/
6. https://docs.github.com/en/developers/webhooks-and-events/webhooks/webhook-events-and-payloads#push 
