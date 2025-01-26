deploy arc
test arc with sample work-flow for 2 repo.
test the upgrade process.
runner name : lab-tes-arc-runner-set
oci://ghcr.io/actions/actions-runner-controller-charts/gha-runner-scale-set-controller



#steps
helm pull oci://ghcr.io/actions/actions-runner-controller-charts/gha-runner-scale-set-controller --version 0.9.3
helm pull oci://ghcr.io/actions/actions-runner-controller-charts/gha-runner-scale-set --version 0.9.3

helm pull oci://ghcr.io/actions/actions-runner-controller-charts/gha-runner-scale-set-controller
helm pull oci://ghcr.io/actions/actions-runner-controller-charts/gha-runner-scale-set
tar -xvf gha-runner-scale-set-controller-0.9.3.tgz
tar -xvf gha-runner-scale-set-0.9.3.tgz
kubectl create ns lab-tes-arc-systems
kubectl create ns lab-tes-arc-runners
cd gha-runner-scale-set-controller/
helm install arc . -n lab-tes-arc-systems ##to install controller
kubectl get all -n lab-tes-arc-systems
kubectl get sa -n lab-tes-arc-systems # copy the sa name and paste into the values.yaml for runner-scale-set
cd ..
pwd
/Users/iafuwape/Downloads/lab-tes-arc-runners/gha-runner-scale-set
helm install lab-tes-arc-runner-set . -n lab-tes-arc-runners --set githubConfigSecret.github_token="clissictoken"

# to update the pat token

helm upgrade lab-tes-arc-runner-set . -n lab-tes-arc-runners --set githubConfigSecret.github_token="newclissictoken"


#first unistall runner scaleset 
helm uninstall lab-tes-arc-runners-set -n lab-tes-arc-runners-new

#second uninstall controller
helm uninstall arc -n lab-tes-arc-systems 

good
#first unistall runner scaleset 
afuwa@LAPTOP-B1MBUT11 MINGW64 ~/Downloads/dev/arc-runners/gha-runner-scale-set (main)
$ helm uninstall lab-tes-arc-runners-set -n lab-tes-arc-runners-new
release "lab-tes-arc-runners-set" uninstalled

#second uninstall controller
afuwa@LAPTOP-B1MBUT11 MINGW64 ~/Downloads/dev/arc-runners/gha-runner-scale-set (main)
$ helm uninstall arc -n lab-tes-arc-systems
release "arc" uninstalled


#third uninstall controller
#use helm command to install controller
$ pwd
/c/Users/afuwa/Downloads/dev/arc-runners-0.9.3/cd gha-runner-scale-set-controller

helm install arc . -n lab-tes-arc-systems ##to install controller

kubectl get all -n lab-tes-arc-systems # confirm no pod installed
kubectl get sa -n lab-tes-arc-systems  # confirm no sa installed

 
#fourth uninstall controller
use helm command to install runner scaleset
pwd
/c/Users/afuwa/Downloads/dev/arc-runners-0.9.3/gha-runner-scale-set
cd gha-runner-scale-set
helm upgrade --install lab-tes-arc-runners-set . -n lab-tes-arc-runners-new --set githubConfigSecret.github_token="classictoken"

claccis token should have workflow, org permission.



