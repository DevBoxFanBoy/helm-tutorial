# helm-tutorial
Shows usage with helm and it's features, demos with minikube.

Show install, upgrade, auto-rollbacks.

Install first time:
* cd helm/wiremock
* kubectl create namespace mockserver
* helm upgrade --install --wait --timeout 60s --namespace mockserver wiremock ./
* helm test -n mockserver wiremock

Impl with bug:
* cat templates/deployment.yaml | sed -s 's/#VERSION#/#VERSION1#/' | tee templates/deployment.yaml
* cat static/mappings/helloMapping.json | sed -s 's/200/500/' | tee static/mappings/helloMapping.json
* helm upgrade --install --wait --timeout 60s --namespace mockserver wiremock ./

Bugfix:
* cat static/mappings/helloMapping.json | sed -s 's/500/200/' | tee static/mappings/helloMapping.json
* cat templates/deployment.yaml | sed -s 's/#VERSION1#/#VERSION#/' | tee templates/deployment.yaml