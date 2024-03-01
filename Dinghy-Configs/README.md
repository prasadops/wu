# WURM-Dinghy-Setup

## YAML files for testing dinghy and for building the image.
Procedure for using dingy:
1) Configure content in dingy.yml in dingy-cm.yaml . In particular:
   -  "templateOrg" (e.g. ksrinimba),
   -  "templateRepo" (e.g. sample-dinghy)
   -  "githubEndpoint" (api.github.com is the default) 
2) update dinghy-git-secret.yaml to include a "username:token" string that has access to the git-repo
   - In the example above, it will access https://api.github.com/ksrinimba/sample-dinghy
   - fork opsmx/sample-dinghy to your own, modify "ksrinimba"
3) The ingress-yaml specified is for nginx, update as required
4) Apply the ConfigMap, Secret, SVC and Deployment YAML, followed by Ingress Yaml
   - check logs of the pod to ensure that configuration loaded is correct
5) Configure web-hook in the git repo to: https://**dinghy-routed-url**/v1/webhooks/github
   - select application/json content-type
   - Just push the event
   - in dinghy-cm.yaml, uncomment "logging" and change it to 'debug' to see the debug messages
6) Either change application/dinghyfile (env: qa) or take one of the previous paylogs and re-post from git.
7) The application and pipelines should get created in Spinnaker. Give it a min or two AFTER github-webhook shows the green-tic and refresh spinnaker.

## Procedure for building the dinghy-oss image:

- git clone https://github.com/armory/dinghy/tree/release-2.20.x
- cd dinghy
- docker build .   SEE THE DOT AT THE END
- docker tag IMAGE_ID quay.io/opsmxpublic/dinghy-oss:20210224
- docker login (to quay.io)
- docker push quay.io/opsmxpublic/dinghy-oss:20210224
  
