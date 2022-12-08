# How to export YAMLs from oc new-app

```bash

oc get deployment/django -o yaml | yq 'del(.metadata.resourceVersion, .metadata.uid, .metadata.creationTimestamp, .metadata.selfLink, .metadata.managedFields, .status, .metadata.namespace, .metadata.generation)' > Deployment.yaml

# Change Deployment config image to:
#           - image: image-registry.openshift-image-registry.svc:5000/dummy/dummy:latest
#  Annotation namesapce löschen:
#            image.openshift.io/triggers: '[{"from":{"kind":"ImageStreamTag","name":"django:latest"},"fieldPath":"spec.template.spec.containers[?(@.name==\"django\")].image","pause":"false"}]'


oc get imagestream/django -o yaml |  yq 'del(.metadata.resourceVersion, .metadata.uid, .metadata.creationTimestamp, .metadata.selfLink, .metadata.managedFields, .status, .metadata.namespace, .metadata.generation)' > ImageStream.yaml

oc get buildconfig/django -o yaml |  yq 'del(.metadata.resourceVersion, .metadata.uid, .metadata.creationTimestamp, .metadata.selfLink, .metadata.managedFields, .status, .metadata.namespace, .metadata.generation)' > BuildConfig.yaml


oc get service/django -o yaml |  yq 'del(.metadata.resourceVersion, .metadata.uid, .metadata.creationTimestamp, .metadata.selfLink, .metadata.managedFields, .status, .metadata.namespace, .metadata.generation,.spec.clusterIP, .spec.clusterIPs)' > Service.yaml

oc get route/django -o yaml |  yq 'del(.metadata.resourceVersion, .metadata.uid,
 .metadata.creationTimestamp, .metadata.selfLink, .metadata.managedFields, .status,
 .metadata.namespace, .metadata.generation, .spec.host)' > Route.yaml

```

# Build & Deploy

```
oc apply -k https://github.com/rbo/kustomize-build-deploy
oc logs bc/django -f

```


```