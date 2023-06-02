# scheduler

## Task 1
Running a K8S cluster and replicate some pipeline example.
### Steps
- Read K8S basics and bring up a k8S cluster using https://microk8s.io/
- Learn Prometheus basics and install it on K8S cluster
- Read about K8S operators
- Install this Operator https://docs.seldon.io/projects/seldon-core/en/v2/index.html and replicate this example: https://docs.seldon.io/projects/seldon-core/en/v2/contents/examples/pipeline-examples.html
### Challanges and logs
- Error: `UPGRADE FAILED: pre-upgrade hooks failed: timed out waiting for the condition during enabling observability`
  - Inspect the `htop` in the tree mode to find the targeted process
  - Change the `/var/snap/microk8s/common/addons/core/addons/observability/enable` script to use `--debug` in order to debug the situation
  - Inspect the new logs of debug status
  - Add `--no-hook` according to `https://github.com/prometheus-community/helm-charts/issues/1041#issuecomment-1285156934`.
  - Done
- Error: `couldn't get resource list for metrics.k8s.io/v1beta1: the server is currently unable to handle the request`
  - Finding https://cloud.google.com/kubernetes-engine/docs/troubleshooting#namespace_stuck_in_terminating_state as a great solution by Google
  - Found out the problem is belonged to Metrics-Server pod, which is not correctly settled up
  - Check Metrics-Server logs using `kubectl describe pod metrics-server-6f754f88d -n kube-system`
  - The man error found: `Metrics-Server Pulling registry.k8s.io/metrics-server 403 forbidden error`
  - Tried to use VPN, did not work.
  - Tried to use 403.online, did not work.
  - Tried to use Shecan.ir, it worked!
