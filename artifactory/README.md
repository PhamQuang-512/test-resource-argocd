# JFrog Artifactory Helm Chart

**IMPORTANT!** Our Helm Chart docs have moved to our main documentation site. Below you will find the basic instructions for installing, uninstalling, and deleting Artifactory. For all other information, refer to [Installing Artifactory](https://www.jfrog.com/confluence/display/JFROG/Installing+Artifactory#InstallingArtifactory-HelmInstallation).

## Prerequisites
* Kubernetes 1.19+
* Artifactory Pro trial license [get one from here](https://www.jfrog.com/artifactory/free-trial/)

## Chart Details
This chart will do the following:

* Deploy Artifactory-Pro/Artifactory-Edge (or OSS/CE if custom image is set)
* Deploy a PostgreSQL database using the stable/postgresql chart (can be changed) **NOTE:** For production grade installations it is recommended to use an external PostgreSQL.
* Deploy an optional Nginx server
* Optionally expose Artifactory with Ingress [Ingress documentation](https://kubernetes.io/docs/concepts/services-networking/ingress/)

## Installing the Chart

### Add JFrog Helm repository

Before installing JFrog helm charts, you need to add the [JFrog helm repository](https://charts.jfrog.io) to your helm client

```bash
helm repo add jfrog https://charts.jfrog.io
helm repo update
```

### Install Chart
To install the chart with the release name `artifactory`:
```bash
helm upgrade --install artifactory jfrog/artifactory --namespace artifactory --create-namespace
```

### High Availability

Note: High availability is only supported with an Artifactory Enterprise license.

To enable high availability (HA) in Artifactory, set the artifactory.replicaCount to 2 or more. A replica count of 3 is recommended for optimal performance and redundancy.

When deploying with artifactory.replicaCount > 1, avoid using artifactory.persistence.type=file-system for the filestore configuration in HA setups, as it may cause data inconsistency.

For more details on configuring the filestore, Refer [here](https://jfrog.com/help/r/jfrog-installation-setup-documentation/filestore-configuration)


```bash
# Start artifactory with 3 replicas
helm upgrade --install artifactory jfrog/artifactory --set artifactory.replicaCount=3,artifactory.persistence.type=cluster-file-system --namespace jfrog-platform --create-namespace
```

### Apply Sizing configurations to the Chart
Note that sizings with more than one replica require an enterprise license for HA . Refer [here](https://jfrog.com/help/r/jfrog-installation-setup-documentation/high-availability)
To apply the chart with recommended sizing configurations :
For small configurations :
```bash
helm upgrade --install artifactory jfrog/artifactory -f sizing/artifactory-small.yaml --namespace artifactory --create-namespace
```

## Uninstalling Artifactory

Uninstall is supported only on Helm v3 and on.

Uninstall Artifactory using the following command.

```bash
helm uninstall artifactory && sleep 90 && kubectl delete pvc -l app=artifactory
```

## Deleting Artifactory

**IMPORTANT:** Deleting Artifactory will also delete your data volumes and you will lose all of your data. You must back up all this information before deletion. You do not need to uninstall Artifactory before deleting it.

To delete Artifactory use the following command.

```bash
helm delete artifactory --namespace artifactory
```
