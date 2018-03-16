# multicluster-sandbox

## Cluster Registry

Is an API server to keep track of and perform operations on multiple k8s cluster

- acts as a hosted `kubeconfig` file for multiple clusters (kubeconfig = a generic name for some file that configures access to one cluster)
- lets you add custom annotations and label filtering for clusters
- support for external controllers watching for cluster updates

There are two modes-- raw `cluster-registry` (API server only), and `crinit` tool, for deploying a cluster registry onto an existing cluster.

### api endpoints

`GET /clusters` - list clusters
`GET /clusters/<name>` - get cluster by name
`GET /watch/clusters`
`GET /watch/clusters/name`

`POST /clusters` - create a cluster
`PUT /clusters/<name>` - replace existing cluster
`PATCH /clusters/<name>` - partially update existing cluster


`DELETE /clusters` - delete cluster collection
`DELETE /clusters/<name>`

### model types

1. `Cluster` = a k8s Object with metadata, plus a `ClusterSpec` and `ClusterStatus`
2. `ClusterSpec` = has 2 things, `KubernetesAPIEndpoints` for where it lives, and `AuthInfo` to get to it
3. `ClusterStatus`= (nothing yet)
4. `ClusterList` = all clusters in registry
4. `KubernetesAPIEndpoints` = API server endpoints for just one Cluster (supports multiple identities)
5. `ServerAddressByClientCIDR` = one cluster API server identity
6. `AuthInfo` = public information (list of Providers) for how to access one cluster (doesn't hold sensitive info)
7. `AuthProviderConfig` = one way to Authenticate into one cluster
8. `AuthProviderType` = metadata about one way to Authenticate into one cluster


### repo contains:

- `api/` swagger spec
- `cmd/`- includes `crinit.go` - spins up api server and backing etcd store (from images)
- `pkg/`
    - `/api` codegen for api swagger spec
      - model types
      - `client`
      - api server implementations for raw cluster-registry, and crinit cmd line tool. spins up an endpoint plus an Etcd store
      - matching between cluster registry API endpoint and k8s API calls
