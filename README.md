# fleet-multus-cni

Fleet bundle to deploy Multus CNI on K3s clusters via Rancher Fleet.

## Structure

```
fleet-multus-cni/
├── crd-adopt/          # Step 1: adopts K3s-owned CRD into Helm
│   ├── fleet.yaml
│   └── adopt-job.yaml
└── multus/             # Step 2: installs rke2-multus (depends on Step 1)
    ├── fleet.yaml
    └── values.yaml
```

## How it works

1. Fleet deploys `crd-adopt` first — a Job that annotates the
   `network-attachment-definitions` CRD (pre-created by K3s/Flannel)
   with Helm ownership metadata.
2. Once `crd-adopt` is Ready, Fleet deploys `rke2-multus` via the
   Rancher Helm chart with K3s-correct CNI paths.

## Add to Fleet

Rancher → Fleet → Git Repos → Add Repository:

| Field          | Value                                        |
|----------------|----------------------------------------------|
| Name           | `fleet-multus-cni`                           |
| Repository URL | `https://github.com/teo-tsou/fleet-multus-cni` |
| Branch         | `main`                                       |
| Paths          | `/`                                          |
