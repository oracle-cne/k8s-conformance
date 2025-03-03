The latest prerequisite and software installation steps are documented in the [Installation](https://github.com/oracle-cne/ocne#installation).
This is a site-specific example to validate conformance and not to be used in production.
***
Release 2.0 of Oracle Cloud Native Environment provides a new way of creating and managing Kubernetes clusters, compared to previous releases. 

1. Use the following example command for a quick start to create a 3 control plane nodes and 2 worker nodes on a local system 
    ~~~
       # ocne cluster start --control-plane-nodes 3 --worker-nodes 2
       # export KUBECONFIG=$(ocne cluster show)
       # kubectl get nodes
       NAME                   STATUS   ROLES           AGE    VERSION
       ocne-control-plane-1   Ready    control-plane   2m12s   v1.31.6+1.el8
       ocne-control-plane-2   Ready    control-plane   92s     v1.31.6+1.el8
       ocne-control-plane-3   Ready    control-plane   92s     v1.31.6+1.el8
       ocne-worker-1          Ready    <none>          93s     v1.31.6+1.el8
       ocne-worker-2          Ready    <none>          93s     v1.31.6+1.el8
    ~~~

2. Obtain sonobuoy
    ~~~
       export SONOBUOY_VERSION="0.57.3"
       export SONOBUOY_FILENAME="sonobuoy_${SONOBUOY_VERSION}_linux_amd64.tar.gz"
       curl -LO https://github.com/vmware-tanzu/sonobuoy/releases/download/v${SONOBUOY_VERSION}/${SONOBUOY_FILENAME}
       tar zxf ${SONOBUOY_FILENAME}
    ~~~
    Begin test
    ~~~
       # ./sonobuoy version --kubeconfig $KUBECONFIG
       Sonobuoy Version: v0.57.3
       MinimumKubeVersion: 1.17.0
       MaximumKubeVersion: 1.99.99
       GitSHA: a988242e8bbded3ef4602eda48addcfac24a1a91
       GoVersion: go1.23.6
       Platform: linux/amd64
       API Version:  v1.31.6+1.el8

       # echo "dockerLibraryRegistry: mirror.gcr.io/library" > conformance-image-config.yaml
       # ./sonobuoy run --sonobuoy-image projects.registry.vmware.com/sonobuoy/sonobuoy:v0.56.16 \
           --systemd-logs-image projects.registry.vmware.com/sonobuoy/systemd-logs:v0.4 \
           --e2e-repo-config conformance-image-config.yaml --mode=certified-conformance
    ~~~
