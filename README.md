# arc-runners
# added github url to line 3
githubConfigUrl: ""
githubConfigUrl: ""

# in C:\Users\afuwa\Downloads\dev\arc-runners\gha-runner-scale-set\values.yaml comment line 189-193

  <!-- spec:
    containers:
      - name: runner
        image: ghcr.io/actions/actions-runner:latest
        command: ["/home/runner/run.sh"] -->

# uncomment 114 - 158

    spec:
      initContainers:
      - name: init-dind-externals
        image: ghcr.io/actions/actions-runner:latest
        command: ["cp", "-r", "-v", "/home/runner/externals/.", "/home/runner/tmpDir/"]
        volumeMounts:
          - name: dind-externals
            mountPath: /home/runner/tmpDir
      containers:
      - name: runner
        image: ghcr.io/actions/actions-runner:latest
        command: ["/home/runner/run.sh"]
        env:
          - name: DOCKER_HOST
            value: unix:///var/run/docker.sock
        volumeMounts:
          - name: work
            mountPath: /home/runner/_work
          - name: dind-sock
            mountPath: /var/run
      - name: dind
        image: docker:dind
        args:
          - dockerd
          - --host=unix:///var/run/docker.sock
          - --group=$(DOCKER_GROUP_GID)
        env:
          - name: DOCKER_GROUP_GID
            value: "123"
        securityContext:
          privileged: true
        volumeMounts:
          - name: work
            mountPath: /home/runner/_work
          - name: dind-sock
            mountPath: /var/run
          - name: dind-externals
            mountPath: /home/runner/externals
      volumes:
      - name: work
        emptyDir: {}
      - name: dind-sock
        emptyDir: {}
      - name: dind-externals
        emptyDir: {}


# uncomment 201 to 203
# controllerServiceAccount:
#   namespace: arc-system
#   name: test-arc-gha-runner-scale-set-controller


change namespace to : lab-tes-arc-systems

#change default runner group to below
runnerGroup: "lab-tes-arc-group"

# uncomment 62 to 67 in C:\Users\afuwa\Downloads\dev\arc-runners\gha-runner-scale-set-controller\values.yaml and indent one line forward
limits:
  cpu: 100m
  memory: 128Mi
requests:
  cpu: 100m
  memory: 128Mi

kubectl create ns lab-tes-arc-runners

#2 currently.
kubectl create ns lab-tes-arc-systems
kubectl create ns lab-tes-arc-runners-new

cd gha-runner-scale-set-controller/

kubectl get all -n lab-tes-arc-systems # confirm no pod installed
kubectl get sa -n lab-tes-arc-systems  # confirm no sa installed

helm install arc . -n lab-tes-arc-systems ##to install controller
kubectl get all -n lab-tes-arc-systems
<!-- kubectl get sa -n lab-tes-arc-systems # copy the sa name and paste into the values.yaml for runner-scale-set -->
kubectl get sa -n lab-tes-arc-systems #confirm sa name is the same as the one in value.yaml( C:\Users\afuwa\Downloads\dev\arc-runners\gha-runner-scale-set\values.yaml)

kubectl describe sa arc-gha-rs-controller -n lab-tes-arc-systems #to show secret is empty

cd ..
cd gha-runner-scale-set
pwd
/Users/iafuwape/Downloads/lab-tes-arc-runners/gha-runner-scale-set

 cat values.yaml | grep githubConfigUrl
## githubConfigUrl is the GitHub url for where you want to configure runners
githubConfigUrl: "https://github.com/lab-tes"

afuwa@LAPTOP-B1MBUT11 MINGW64 ~/Downloads/dev/arc-runners/gha-runner-scale-set (main)
$ cat /c/Users/afuwa/Downloads/dev/arc-runners/gha-runner-scale-set/values.yaml | grep githubConfigUrl
## githubConfigUrl is the GitHub url for where you want to configure runners
githubConfigUrl: "https://github.com/lab-tes"


helm install lab-tes-arc-runners-0 . -n lab-tes-arc-runners-new --set githubConfigSecret.github_token="classictoken"

helm install lab-tes-arc-runners-set . -n lab-tes-arc-runners-new --set githubConfigSecret.github_token="classictoken"
helm upgrade lab-tes-arc-runners-set . -n lab-tes-arc-runners-new --set githubConfigSecret.github_token="classictoken"
or
helm upgrade --install lab-tes-arc-runners-set . -n lab-tes-arc-runners-new --set githubConfigSecret.github_token="classictoken"


helm install lab-tes-arc-runner-set . -n lab-tes-arc-runners --set githubConfigSecret.github_token="classictoken"

# to update the pat token

helm upgrade lab-devsecops-arc-runner-set . -n lab-devsecops-arc-runners --set githubConfigSecret.github_token="newclissictoken"

afuwa@LAPTOP-B1MBUT11 MINGW64 ~/Downloads/dev/arc-runners/gha-runner-scale-set-controller (main)

$ helm uninstall lab-tes-arc-runner-set -n lab-tes-arc-runners
release "lab-tes-arc-runner-set" uninstalled

 

helm install lab-tes-arc-runner-set . -n lab-tes-arc-runners --set githubConfigSecret.github_token="classictoken"

 

helm ls -n lab-tes-arc-runners-new

helm uninstall -n lab-tes-arc-runners-new lab-tes-arc-runner-set-10 lab-tes-arc-runners
$ kubectl get rolebinding -n lab-tes-arc-systems -o yaml