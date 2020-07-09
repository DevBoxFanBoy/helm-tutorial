# helm-tutorial
Shows usage with helm and it's features, demos with minikube.

Show install, upgrade, auto-rollbacks.

Install first time:
* cd helm/wiremock
* kubectl create namespace mockserver
* helm upgrade --install --wait --timeout 60s --namespace mockserver wiremock ./
* helm test -n mockserver wiremock

Impl with bug:
* cat templates/deployment.yaml | sed -s 's/2.27.0/2.27.1/' | tee templates/deployment.yaml
* cat templates/tests/smoketest.yaml | sed -s 's#/hello#/break#' | tee templates/tests/smoketest.yaml
* helm upgrade --install --wait --timeout 60s --namespace mockserver wiremock ./

Bugfix:
* cat templates/tests/smoketest.yaml | sed -s 's#/break#/hello#' | tee templates/tests/smoketest.yaml
* cat templates/deployment.yaml | sed -s 's/2.27.1/latest/' | tee templates/deployment.yaml
* helm upgrade --install --wait --timeout 60s --namespace mockserver wiremock ./

Changes on smoketest only:
* cat templates/tests/smoketestJob.yaml | sed -s 's#/hello#/break#' | tee templates/tests/smoketestJob.yaml
* helm upgrade --install --wait --timeout 60s --namespace mockserver wiremock ./
* helm test -n mockserver wiremock
* cat templates/tests/smoketestJob.yaml | sed -s 's#/break#/hello#' | tee templates/tests/smoketestJob.yaml
* helm upgrade --install --wait --timeout 60s --namespace mockserver wiremock ./
* helm test -n mockserver wiremock

---
Notes:
Every change in the deployment.yaml create new pods.
These pods will replace the old release until all new pods are stable without errors and all tests passed successfully.

Changes on tests only, does not affect the deployment!
This only affects the changed resource eg. `smoketestJob.yaml`.