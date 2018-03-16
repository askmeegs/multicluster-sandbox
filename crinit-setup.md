## Setting up Cluster-Registry (Minikube / Mac)


### `crinit` Standalone

1. `minikube start `
2. install bazel for mac
3. build `crinit` for mac: `bazel build //cmd/crinit`
4. run the root `crinit` command to make sure it built: `bazel-bin/cmd/crinit/darwin_amd64_stripped/crinit`
5. init standalone cluster registry: `bazel-bin/cmd/crinit/darwin_amd64_stripped/crinit standalone init minireg --host-cluster-context=minikube`
6. you can now `GET /clusters` as a custom k8s API object:

```
kubectl get clusters --context minireg

```
