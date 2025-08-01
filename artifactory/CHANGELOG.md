# JFrog Artifactory Chart Changelog
All changes to this chart will be documented in this file.

## [107.117.5] - June 23, 2025
* Added support for AWS S3 V3 credentials (identity and credential) from a Kubernetes Secret
* Added support for Azure credentials (accountName and accountKey) from a Kubernetes Secret
* Fixed Artifactory nodePort misplaced
* Added High Availability section to README.md
* Note: `splitServicesToContainers: true` has been the default setting, and starting with releases after september 2025, the helm chart will no longer support disabling this option

## [107.111.0] - April 15, 2025
* Added custom HPA metrics support for Artifactory service `autoscaling.metrics`
* Updated rtfs version to `1.6.13`
* Added a default resource preset for PostgreSQL as small
* Updated postgresql upgrade warnings
* Added new JFConfig service
* Enabled Evidence service by default to true
* Note added: `artifactory.persistence.existingClaim` is supported only in standalone mode. It is not supported in HA mode with the Artifactory chart.

## [107.109.0] - March 04, 2025
* Added a condition to enable evidence only on pro chart
* Adjusted Nginx settings for better high-scale efficiency
* Upgrade postgres image to 16.6.0-debian-12-r2
* **Breaking changes**
* Upgrade postgres chart version to 15.5.20
  * This has many changes related to key names and path in values yaml
  * The effected keys present in default yaml have been aligned to the new path in 15.5.20
  * if you have customised any keys, make sure to validate it with the 15.5.20 chart
  * Support to change postgres admin username is removed in postgres chart, will be postgres
  * Delete the postgresql statefulset and postgresql secret before the upgrade. for more information, please refer the postgresql upgrade [docs](https://github.com/bitnami/charts/tree/main/bitnami/postgresql#how-to-upgrade-to-version-1100)

## [107.107.0] - Feb 17, 2025
* Added support for json based console logging `.Values.logToStdoutJson`
* Added `customPersistentVolumeClaim` as mount for all containers
* Added a condition to enable onemodel only on pro chart

## [107.104.0] - Feb 18, 2025
* Added new RTFS service
* Added new Topology service
* Added new Onemodel service
* Added `artifactory.servicePrependReleaseName` support to prepend release name to fix mismatch between statefulSet and service
* Added customVolumeMounts support for frontend,event,evidence,jfconnect,topology,observability containers
* Added ingress and nginx routing support for rtfs service context
* Added recommended sizing extraEnvironmentVariables for access container
* Added default extra javaOpts support in system yaml for topology
* Modified the RTFS chart and the topology probe values
* Fixed secret based annotations for RTFS deployment
* Fixed `shared` block in system.yaml to include all properties
* Added size based resources for evidence service and rtfs service
* Fixed RTFS jfrogUrl issue for platform chart
* Added hpa support for RTFS service
* Updated rtfs version to `1.5.26`
* Fixed disabling onemodel using `onemodel.enabled=false`
* Removed unwanted database support from rtfs

## [107.102.0] - Nov 26, 2024
* Remove the Xms and Xmx with InitialRAMPercentage and MaxRAMPercentage if they are available in extra_java_options

## [107.98.0] - Nov 06, 2024
* Add support for `extraEnvironmentVariables` on filebeat Sidecar [GH-1377](https://github.com/jfrog/charts/pull/1377)
* Support for SSL offload HTTPS proto override in Nginx service (ClusterIP, LoadBalancer) layer. Introduced `nginx.service.ssloffloadForceHttps` field with boolean type. [GH-1906](https://github.com/jfrog/charts/pull/1906)
* Enable Access workers integration when artifactory.worker.enabled is true
* Added `signedUrlExpirySeconds` option to artifactory.persistence.type of `google-storage`, `google-storage-v2`, and `google-storage-v2-direct` [GH-1858](https://github.com/jfrog/charts/pull/1858)
* Added support to bootstrap jfconnect custom certs to jfconnect trusted directory
* Fixed the type of `.Values.artifactory.persistence.googleStorage.signedUrlExpirySeconds` in binarystore.xml from boolean to integer
* Added support for modifying `pathType` in ingress

## [107.96.0] - Sep 10, 2024
* Merged Artifactory sizing templates to a single file per size

## [107.94.0] - Aug 14, 2024
* Fixed #Expose rtfs port only when it is enabled

## [107.93.0] - Aug 9, 2024
* Added support for worker via `artifactory.worker.enabled` flag
* Fixed creation of duplicate objects in statefulSet

## [107.92.0] - July 31, 2024
* Updating the example link for downloading the DB driver
* Adding dedicated ingress and service path for GRPC protocol

## [107.91.0] - July 18, 2024
* Remove X-JFrog-Override-Base-Url port when using default `443/80` ports

## [107.90.0] - July 18, 2024
* Fixed #adding colon in image registry which breaks deployment [GH-1892](https://github.com/jfrog/charts/pull/1892)
* Added new `nginx.hosts` to use Nginx server_name directive instead of `ingress.hosts`
* Added a deprecation notice of ingress.hosts when `ngnix.enabled` is true
* Added new evidence service
* Corrected database connection values based on sizing
* **IMPORTANT**
* Separate access from artifactory tomcat to run on its own dedicated tomcat
  * With this change access will be running in its own dedicated container
  * This will give the ability to control resources and java options specific to access
    Can be done by passing the following,
    `access.javaOpts.other`
    `access.resources`
    `access.extraEnvironmentVariables`
* Added Binary Provider recommendations

## [107.89.0] - June 7, 2024
* Fix the indentation of the commented-out sections in the values.yaml file
* Fixed sizing values by removing `JF_SHARED_NODE_HAENABLED` in xsmall/small configurations

## [107.88.0] - May 29, 2024
* **IMPORTANT**
* Refactored `nginx.artifactoryConf` and `nginx.mainConf` configuration (moved to files/nginx-artifactory-conf.yaml  and files/nginx-main-conf.yaml instead of keys in values.yaml)

## [107.87.0] - May 29, 2024
* Renamed `.Values.artifactory.openMetrics` to `.Values.artifactory.metrics`

## [107.85.0] - May 29, 2024
* Changed `migration.enabled` to false by default. For 6.x to 7.x migration, this flag needs to be set to `true`

## [107.84.0] - May 29, 2024
* Added image section for `initContainers` instead of `initContainerImage`
* Renamed `router.image.imagePullPolicy` to `router.image.pullPolicy`
* Removed image section for `loggers`
* Added support for `global.verisons.initContainers` to override `initContainers.image.tag`
* Fixed an issue with extraSystemYaml merge
* **IMPORTANT**
* Renamed `artifactory.setSecurityContext` to `artifactory.podSecurityContext` 
* Renamed `artifactory.uid` to `artifactory.podSecurityContext.runAsUser`
* Renamed `artifactory.gid` to `artifactory.podSecurityContext.runAsGroup` and `artifactory.podSecurityContext.fsGroup`
* Renamed `artifactory.fsGroupChangePolicy` to `artifactory.podSecurityContext.fsGroupChangePolicy`
* Renamed `artifactory.seLinuxOptions` to `artifactory.podSecurityContext.seLinuxOptions`
* Added flag `allowNonPostgresql` defaults to false
* Update postgresql tag version to `15.6.0-debian-12-r5`
* Added a check if `initContainerImage` exists
* Fixed an issue to generate unified secret to support artifactory fullname [GH-1882](https://github.com/jfrog/charts/issues/1882)
* Fixed an issue template render on loggers [GH-1883](https://github.com/jfrog/charts/issues/1883)
* Fixed resource constraints for "setup" initContainer of nginx deployment [GH-962] (https://github.com/jfrog/charts/issues/962)
* Added .Values.artifactory.unifiedSecretPrependReleaseName` for unified secret to prepend release name
* Fixed maxCacheSize and cacheProviderDir mix up under azure-blob-storage-v2-direct template in binarystore.xml

## [107.82.0] - Mar 04, 2024
* Added `disableRouterBypass` flag as experimental feature, to disable the artifactoryPath /artifactory/ and route all traffic through the Router.
* Removed Replicator service

## [107.81.0] - Feb 20, 2024
* **IMPORTANT**
* Refactored systemYaml configuration (moved to files/system.yaml instead of key in values.yaml)
* Added ability to provide `extraSystemYaml` configuration in values.yaml which will merge with the existing system yaml when `systemYamlOverride` is not given [GH-1848](https://github.com/jfrog/charts/pull/1848)
* Added option to modify the new cache configs, maxFileSizeLimit and skipDuringUpload
* Added IPV4/IPV6 Dualstack flag support for Artifactory and nginx service
* Added `singleStackIPv6Cluster` flag, which manages the Nginx configuration to enable listening on IPv6 and proxying.
* Fixing broken link for creating additional kubernetes resources. Refer [here](https://github.com/jfrog/log-analytics-prometheus/blob/master/helm/artifactory-values.yaml)
* Refactored installerInfo configuration (moved to files/installer-info.json instead of key in values.yaml)

## [107.80.0] - Feb 20, 2024
* Updated README.md to create a namespace using `--create-namespace` as part of helm install

## [107.79.0] - Feb 20, 2024
* **IMPORTANT**
* Added `unifiedSecretInstallation` flag which enables single unified secret holding all internal (chart) secrets to `true` by default
* Added support for azure-blob-storage-v2-direct config
* Added option to set Nginx to write access_log to container STDOUT
* **Important change:**
* Update postgresql tag version to `15.2.0-debian-11-r23`
* If this is a new deployment or you already use an external database (`postgresql.enabled=false`), these changes **do not affect you**!
* If this is an upgrade and you are using the default bundles PostgreSQL (`postgresql.enabled=true`), you need to pass previous 9.x/10.x/12.x/13.x's postgresql.image.tag, previous postgresql.persistence.size and databaseUpgradeReady=true

## [107.77.0] - April 22, 2024
* Removed integration service
* Added recommended postgresql sizing configurations under sizing directory
* Updated artifactory-federation (probes, port, embedded mode)
* Fixed - Removed duplicate keys of the sizing yaml file
* Fixing broken nginx port [GH-1860](https://github.com/jfrog/charts/issues/1860)
* Added nginx.customCommand to use custom commands for the nginx container

## [107.76.0] - Dec 13, 2023
* Added connectionTimeout and socketTimeout paramaters under AWSS3 binarystore section
* Reduced nginx startupProbe initialDelaySeconds

## [107.74.0] - Nov 30, 2023
* Added recommended sizing configurations under sizing directory, please refer [here](README.md/#apply-sizing-configurations-to-the-chart)
* **IMPORTANT**
* Added min kubeVersion ">= 1.19.0-0" in chart.yaml

## [107.70.0] - Nov 30, 2023
* Fixed - StatefulSet pod annotations changed from range to toYaml [GH-1828](https://github.com/jfrog/charts/issues/1828)
* Fixed - Invalid format for awsS3V3 `multiPartLimit,multipartElementSize` in binarystore.xml.
* Fixed - SecurityContext with runAsGroup in artifactory [GH-1838](https://github.com/jfrog/charts/issues/1838)
* Added support for custom labels in the Nginx pods [GH-1836](https://github.com/jfrog/charts/pull/1836)
* Added podSecurityContext and containerSecurityContext for nginx
* Added support for nginx on openshift, set `podSecurityContext` and `containerSecurityContext` to false
* Renamed nginx internalPort 80,443 to 8080,8443 to support openshift

## [107.69.0] - Sep 18, 2023
* Adjust rtfs context
* Fixed - Metadata service does not respect customVolumeMounts for DB CAs [GH-1815](https://github.com/jfrog/charts/issues/1815)

## [107.68.8] - Sep 18, 2023
* Reverted - Enabled `unifiedSecretInstallation` by default [GH-1819](https://github.com/jfrog/charts/issues/1819)
* Removed openshift condition check from NOTES.txt

## [107.68.7] - Aug 28, 2023
* Enabled `unifiedSecretInstallation` by default

## [107.67.0] - Aug 28, 2023
* Add 'extraJavaOpts' and 'port' values to federation service

## [107.66.0] - Aug 28, 2023
* Added federation service container in artifactory
* Add rtfs service to ingress in artifactory

## [107.64.0] - Aug 28, 2023
* Added support to configure event.webhooks within generated system.yaml
* Fixed an issue to generate ssl certificate should support artifactory fullname
* Added binarystore.xml template to persistence storage type `nfs`. The default Filestore location configured according to artifactory.persistence.nfs.dataDir.
* Added 'multiPartLimit' and 'multipartElementSize' parameters to awsS3V3 binary providers.
* Increased default Artifactory Tomcat acceptCount config to 400
* Fixed Illegal Strict-Transport-Security header in nginx config

## [107.63.0] - Aug 28, 2023
* Added support for Openshift by adding the securityContext in container level.
* **IMPORTANT**
* Disable securityContext in container and pod level to deploy postgres on openshift.
* Fixed support for fsGroup in non openshift environemnt and runAsGroup in openshift environment.
* Fixed - Helm Template Error when using artifactory.loggers [GH-1791](https://github.com/jfrog/charts/issues/1791)
* Removed the nginx disable condition for openshift
* Fixed jfconnect disabling as micro-service on splitcontainers [GH-1806](https://github.com/jfrog/charts/issues/1806)

## [107.62.0] - Jun 5, 2023
* Upgraded to autoscaling/v2
* Added support for 'port' and 'useHttp' parameters for s3-storage-v3 binary provider [GH-1767](https://github.com/jfrog/charts/issues/1767)

## [107.61.0] - May 31, 2023
* Added new binary provider `google-storage-v2-direct`
* Added missing parameter 'enableSignedUrlRedirect' to 'googleStorage'

## [107.60.0] - May 31, 2023
* Enabled `splitServicesToContainers` to true by default
* Updated the recommended values for small, medium and large installations to support the 'splitServicesToContainers'

## [107.59.0] - May 31, 2023
* Fixed reference of `terminationGracePeriodSeconds`
* Added Support for Cold Artifact Storage as part of the systemYaml configuration (disabled by default)
* Added new binary provider `s3-storage-v3-archive`
* Fixed jfconnect disabling as micro-service on non-splitcontainers
* Fixed wrong cache-fs provider ID of cluster-s3-storage-v3 in the binarystore.xml [GH-1772](https://github.com/jfrog/charts/issues/1772)

## [107.58.0] - Mar 23, 2023
* Updated postgresql multi-arch tag version to `13.10.0-debian-11-r14`
* Removed obselete remove-lost-found initContainer`
* Added env JF_SHARED_NODE_HAENABLED under frontend when running in the container split mode 

## [107.57.0] - Mar 02, 2023
* Updated initContainerImage and logger image to `ubi9/ubi-minimal:9.1.0.1793`

## [107.55.0] - Jan 31, 2023
* Updated initContainerImage and logger image to `ubi9/ubi-minimal:9.1.0.1760`
* Adding a custom preStop to Artifactory router for allowing graceful termination to complete

## [107.53.0] - Jan 20, 2023
* Updated initContainerImage and logger image to `ubi8/ubi-minimal:8.7.1049`

## [107.50.0] - Jan 20, 2023
* Updated postgresql tag version to `13.9.0-debian-11-11`
* Fixed an issue for capabilities check of ingress
* Updated jfrogUrl text path in migrate.sh file
* Added a note that from 107.46.x chart versions, `copyOnEveryStartup` is not needed for binarystore.xml, it is always copied via initContainers. For more Info, Refer [GH-1723](https://github.com/jfrog/charts/issues/1723)

## [107.49.0] - Jan 16, 2023
* Added support for setting `seLinuxOptions` in `securityContext` [GH-1699](https://github.com/jfrog/charts/pull/1699)
* Added option to enable/disable proxy_request_buffering and proxy_buffering_off [GH-1686](https://github.com/jfrog/charts/pull/1686)
* Updated initContainerImage and logger image to `ubi8/ubi-minimal:8.7.1049`

## [107.48.0] - Oct 27, 2022
* Updated router version to `7.51.0`

## [107.47.0] - Sep 29, 2022
* Updated initContainerImage to `ubi8/ubi-minimal:8.6-941`
* Added support for annotations for artifactory statefulset and nginx deployment [GH-1665](https://github.com/jfrog/charts/pull/1665)
* Updated router version to `7.49.0`

## [107.46.0] - Sep 14, 2022
* **IMPORTANT**
* Added support for lifecycle hooks for all containers, changed `artifactory.postStartCommand` to `.Values.artifactory.lifecycle.postStart.exec.command`
* Updated initContainerImage to `ubi8/ubi-minimal:8.6-902`
* Update nginx configuration to allow websocket requests when using pipelines
* Fixed an issue to allow artifactory to make direct API calls to store  instead via jfconnect service when `splitServicesToContainers=true`
* Refactor binarystore.xml configuration (moved to `files/binarystore.xml` instead of key in values.yaml)
* Added new binary providers `cluster-s3-storage-v3`, `s3-storage-v3-direct`, `azure-blob-storage-direct`, `google-storage-v2`
* Deprecated (removed) `aws-s3` binary provider [JetS3t library](https://www.jfrog.com/confluence/display/JFROG/Configuring+the+Filestore#ConfiguringtheFilestore-BinaryProvider)
* Deprecated (removed) `google-storage` binary provider and force persistence storage type `google-storage` to work with `google-storage-v2` only
* Copy binarystore.xml in init Container to fix existing persistence on file system in clear text
* Removed obselete `.Values.artifactory.binarystore.enabled` key
* Removed `newProbes.enabled`, default to new probes
* Added nginx.customCommand using inotifyd to reload nginx's config upon ssl secret or configmap changes [GH-1640](https://github.com/jfrog/charts/pull/1640)

## [107.43.0] - Aug 25, 2022
* Added flag `artifactory.replicator.ingress.enabled` to enable/disable ingress for replicator
* Updated initContainerImage to `ubi8/ubi-minimal:8.6-854`
* Updated router version to `7.45.0`
* Added flag `artifactory.schedulerName` to set for the pods the value of schedulerName field [GH-1606](https://github.com/jfrog/charts/issues/1606)
* Enabled TLS based on access or router in values.yaml

## [107.42.0] - Aug 25, 2022
* Enabled database creds secret to use from unified secret
* Updated router version to `7.42.0`
* Fix duplicate volumes for userPluginSecrets [GH-1650] (https://github.com/jfrog/charts/issues/1650)
* Added support to truncate (> 63 chars) for unifiedCustomSecretVolumeName

## [107.41.0] - June 27, 2022
* Added support for nginx.terminationGracePeriodSeconds [GH-1645](https://github.com/jfrog/charts/issues/1645)
* Use an alternate command for `find` to copy custom certificates
* Added support for circle of trust using `circleOfTrustCertificatesSecret` secret name [GH-1623](https://github.com/jfrog/charts/pull/1623)

## [107.40.0] - June 16, 2022
* Added support for PodDisruptionBudget [GH-1618](https://github.com/jfrog/charts/issues/1618)
* From artifactory 7.38.x, joinKey can be retrived from Admin > User Management > Settings in UI
* Allow templating for pod annotations [GH-1634](https://github.com/jfrog/charts/pull/1634)
* Fixed `customPersistentPodVolumeClaim` name to `customPersistentVolumeClaim`
* Added flags to control enable/disable infra services in splitServicesToContainers

## [107.39.0] - May 31, 2022
* Fix default `artifactory.async.corePoolSize` [GH-1612](https://github.com/jfrog/charts/issues/1612)
* Added support of nginx annotations
* Reduce startupProbe `initialDelaySeconds`
* Align all liveness and readiness probes failureThreshold to `5` seconds
* Added new flag `unifiedSecretInstallation` to enables single unified secret holding all the artifactory secrets
* Updated router version to `7.38.0`
* Add support for NFS config with directories `haBackupDir` and `haDataDir`
* Fixed - disable jfconnect on oss/jcr/cpp flavours [GH-1630](https://github.com/jfrog/charts/issues/1630)

## [107.38.0] - May 04, 2022
* Added support for `global.nodeSelector` to artifactory and nginx pods
* Updated router version to `7.36.1`
* Added support for custom global probes timeout
* Updated frontend container command
* Added topologySpreadConstraints to artifactory and nginx, and add lifecycle hooks to nginx [GH-1596](https://github.com/jfrog/charts/pull/1596)
* Added support of extraEnvironmentVariables for all infra services containers
* Enabled the consumption (jfconnect) flag by default
* Fix jfconnect disabling on non-splitcontainers

## [107.37.0] - Mar 08, 2022
* Added support for customPorts in nginx deployment
* Bugfix - Wrong proxy_pass configurations for /artifactory/ in the default artifactory.conf
* Added signedUrlExpirySeconds option to artifactory.persistence.type aws-S3-V3
* Updated router version to `7.35.0`
* Added useInstanceCredentials,enableSignedUrlRedirect option to google-storage-v2
* Changed dependency charts repo to `charts.jfrog.io`

## [107.36.0] - Mar 03, 2022
* Remove pdn tracker which starts replicator service
* Added silent option for curl probes
* Added readiness health check for the artifactory container for k8s version < 1.20
* Fix property file migration issue to system.yaml 6.x to 7.x

## [107.35.0] - Feb 08, 2022
* Updated router version to `7.32.1`

## [107.33.0] - Jan 11, 2022
* Add more user friendly support for anti-affinity
* Pod anti-affinity is now enabled by default (soft rule)
* Readme fixes
* Added support for setting `fsGroupChangePolicy`
* Added nginx customInitContainers, customVolumes, customSidecarContainers [GH-1565](https://github.com/jfrog/charts/pull/1565)
* Updated router version to `7.30.0`

## [107.32.0] - Dec 22, 2021
* Updated logger image to `jfrog/ubi-minimal:8.5-204`
* Added default `8091` as `artifactory.tomcat.maintenanceConnector.port` for probes check
* Refactored probes to replace httpGet probes with basic exec + curl
* Refactored `database-creds` secret to create only when database values are passed
* Added new endpoints for probes `/artifactory/api/v1/system/liveness` and `/artifactory/api/v1/system/readiness`
* Enabled `newProbes:true` by default to use these endpoints
* Fix filebeat sidecar spool file permissions
* Updated filebeat sidecar container to `7.16.2`

## [107.31.0] - Dec 17, 2021
* Added support for HorizontalPodAutoscaler apiVersion `autoscaling/v2beta2`
* Remove integration service feature flag to make it mandatory service
* Update postgresql tag version to `13.4.0-debian-10-r39`
* Fixed `artifactory.resources` indentation in `migration-artifactory` init container [GH-1562](https://github.com/jfrog/charts/issues/1562)
* Refactored `router.requiredServiceTypes` to support platform chart

## [107.30.0] - Nov 30, 2021
* Fixed incorrect permission for filebeat.yaml
* Updated healthcheck (liveness/readiness) api for integration service
* Disable readiness health check for the artifactory container when running in the container split mode
* Ability to start replicator on enabling pdn tracker

## [107.29.0] - Nov 26, 2021
* Added integration service container in artifactory
* Add support for Ingress Class Name in Ingress Spec [GH-1516](https://github.com/jfrog/charts/pull/1516)
* Fixed chart values to use curl instead of wget [GH-1529](https://github.com/jfrog/charts/issues/1529)
* Updated nginx config to allow websockets when pipelines is enabled
* Moved router.topology.local.requireqservicetypes from system.yaml to router as environment variable
* Added jfconnect in system.yaml
* Updated artifactory container’s health probes to use artifactory api on rt-split
* Updated initContainerImage to `jfrog/ubi-minimal:8.5-204`
* Updated router version to `7.28.2`
* Set Jfconnect enabled to `false` in the artifactory container when running in the container split mode

## [107.28.0] - Nov 11, 2021
* Added default values cpu and memeory in initContainers
* Updated router version to `7.26.0`
* Updated (`rbac.create` and `serviceAccount.create` to false by default) for least privileges
* Fixed incorrect data type for `Values.router.serviceRegistry.insecure` in default values.yaml [GH-1514](https://github.com/jfrog/charts/pull/1514/files)
* **IMPORTANT**
* Changed init-container images from `alpine` to `ubi8/ubi-minimal`
* Added support for AWS License Manager using `.Values.aws.licenseConfigSecretName`

## [107.27.0] - Oct 6, 2021
* **Breaking change**
* Aligned probe structure (moved probes variables under config block)
* Added support for new probes(set to false by default)
* Bugfix - Invalid format for `multiPartLimit,multipartElementSize,maxCacheSize` in binarystore.xml [GH-1466](https://github.com/jfrog/charts/issues/1466)
* Added missioncontrol container in artifactory
* Dropped NET_RAW capability for the containers
* Added resources to migration-artifactory init container
* Added resources to all rt split containers
* Updated router version to `7.25.1`
* Added support for Ingress networking.k8s.io/v1/Ingress for k8s >=1.22 [GH-1487](https://github.com/jfrog/charts/pull/1487)
* Added min kubeVersion ">= 1.14.0-0" in chart.yaml
* Update alpine tag version to `3.14.2`
* Update busybox tag version to `1.33.1`
* Artifactory chart support for cluster license

## [107.26.0] - Aug 23, 2021
* Added Observability container (only when `splitServicesToContainers` is enabled)
* Support for high availability (when replicaCount > 1)
* Added min kubeVersion ">= 1.12.0-0" in chart.yaml

## [107.25.0] - Aug 13, 2021
* Updated readme of chart to point to wiki. Refer [Installing Artifactory](https://www.jfrog.com/confluence/display/JFROG/Installing+Artifactory)
* Added startupProbe and livenessProbe for RT-split containers
* Updated router version to 7.24.1
* Added security hardening fixes
* Enabled startup probes for k8s >= 1.20.x
* Changed network policy to allow all ingress and egress traffic
* Added Observability changes
* Added support for global.versions.router (only when `splitServicesToContainers` is enabled)

## [107.24.0] - July 27, 2021
* Support global and product specific tags at the same time
* Added support for artifactory containers split

## [107.23.0] - July 8, 2021
* Bug fix - logger sideCar picks up Wrong File in helm
* Allow filebeat metrics configuration in values.yaml

## [107.22.0] - July 6, 2021
* Update alpine tag version to `3.14.0`
* Added `nodePort` support to artifactory-service and nginx-service templates
* Removed redundant `terminationGracePeriodSeconds` in statefulset
* Increased `startupProbe.failureThreshold` time

## [107.21.3] - July 2, 2021
* Added ability to change sendreasonphrase value in server.xml via system yaml

## [107.19.3] - May 20, 2021
* Fix broken support for startupProbe for k8s < 1.18.x
* Added support for `nameOverride` and `fullnameOverride` in values.yaml

## [107.18.6] - April 29, 2021
* Bumping chart version to align with app version
* Add `securityContext` option on nginx container

## [12.0.0] - April 22, 2021
* **Breaking change:**
* Increased default postgresql persistence  size to `200Gi` 
* Update postgresql tag version to `13.2.0-debian-10-r55`
* Update postgresql chart version to `10.3.18` in chart.yaml - [10.x Upgrade Notes](https://github.com/bitnami/charts/tree/master/bitnami/postgresql#to-1000)
* If this is a new deployment or you already use an external database (`postgresql.enabled=false`), these changes **do not affect you**!
* If this is an upgrade and you are using the default PostgreSQL (`postgresql.enabled=true`), you need to pass previous 9.x/10.x/12.x's postgresql.image.tag, previous postgresql.persistence.size and databaseUpgradeReady=true
* **IMPORTANT**
* This chart is only helm v3 compatible.
* Fixed filebeat-configmap naming
* Explicitly set ServiceAccount `automountServiceAccountToken` to 'true'
* Update alpine tag version to `3.13.5`

## [11.13.2] - April 15, 2021
* Updated Artifactory version to 7.17.9 - [Release Notes](https://www.jfrog.com/confluence/display/JFROG/Artifactory+Release+Notes#ArtifactoryReleaseNotes-Artifactory7.17.9)

## [11.13.1] - April 6, 2021
* Updated Artifactory version to 7.17.6 - [Release Notes](https://www.jfrog.com/confluence/display/JFROG/Artifactory+Release+Notes#ArtifactoryReleaseNotes-Artifactory7.17.6)
* Update alpine tag version to `3.13.4`

## [11.13.0] - April 5, 2021
* **IMPORTANT**
* Added `charts.jfrog.io` as default JFrog Helm repository
* Updated Artifactory version to 7.17.5 - [Release Notes](https://www.jfrog.com/confluence/display/JFROG/Artifactory+Release+Notes#ArtifactoryReleaseNotes-Artifactory7.17.5)

## [11.12.2] - Mar 31, 2021
* Updated Artifactory version to 7.17.4 - [Release Notes](https://www.jfrog.com/confluence/display/JFROG/Artifactory+Release+Notes#ArtifactoryReleaseNotes-Artifactory7.17.4)

## [11.12.1] - Mar 30, 2021
* Updated Artifactory version to 7.17.3
* Add `timeoutSeconds` to all exec probes - Please refer [here](https://kubernetes.io/docs/tasks/configure-pod-container/configure-liveness-readiness-startup-probes/#configure-probes)

## [11.12.0] - Mar 24, 2021
* Updated Artifactory version to 7.17.2
* Optimized startupProbe time

## [11.11.0] - Mar 18, 2021
* Add support to startupProbe

## [11.10.0] - Mar 15, 2021
* Updated Artifactory version to 7.16.3

## [11.9.5] - Mar 09, 2021
* Added HSTS header to nginx conf

## [11.9.4] - Mar 9, 2021
* Removed bintray URL references in the chart

## [11.9.3] - Mar 04, 2021
* Updated Artifactory version to 7.15.4 - [Release Notes](https://www.jfrog.com/confluence/display/JFROG/Artifactory+Release+Notes#ArtifactoryReleaseNotes-Artifactory7.15.4)

## [11.9.2] - Mar 04, 2021
* Fixed creation of nginx-certificate-secret when Nginx is disabled

## [11.9.1] - Feb 19, 2021
* Update busybox tag version to `1.32.1`

## [11.9.0] - Feb 18, 2021
* Updated Artifactory version to 7.15.3 - [Release Notes](https://www.jfrog.com/confluence/display/JFROG/Artifactory+Release+Notes#ArtifactoryReleaseNotes-Artifactory7.15.3)
* Add option to specify update strategy for Artifactory statefulset

## [11.8.1] - Feb 11, 2021
* Exposed "multiPartLimit" and "multipartElementSize" for the Azure Blob Storage Binary Provider

## [11.8.0] - Feb 08, 2021
* Updated Artifactory version to 7.12.8 - [Release Notes](https://www.jfrog.com/confluence/display/JFROG/Artifactory+Release+Notes#ArtifactoryReleaseNotes-Artifactory7.12.8)
* Support for custom certificates using secrets
* **Important:** Switched docker images download from `docker.bintray.io` to `releases-docker.jfrog.io`
* Update alpine tag version to `3.13.1`

## [11.7.8] - Jan 25, 2021
* Add support for hostAliases

## [11.7.7] - Jan 11, 2021
* Fix failures when using creds file for configurating google storage

## [11.7.6] - Jan 11, 2021
* Updated Artifactory version to 7.12.6 - [Release Notes](https://www.jfrog.com/confluence/display/JFROG/Artifactory+Release+Notes#ArtifactoryReleaseNotes-Artifactory7.12.6)

## [11.7.5] - Jan 07, 2021
* Added support for optional tracker dedicated ingress `.Values.artifactory.replicator.trackerIngress.enabled` (defaults to false)

## [11.7.4] - Jan 04, 2021
* Fixed gid support for statefulset

## [11.7.3] - Dec 31, 2020
* Added gid support for statefulset
* Add setSecurityContext flag to allow securityContext block to be removed from artifactory statefulset

## [11.7.2] - Dec 29, 2020
* **Important:** Removed `.Values.metrics` and `.Values.fluentd` (Fluentd and Prometheus integrations)
* Add support for creating additional kubernetes resources - [refer here](https://github.com/jfrog/log-analytics-prometheus/blob/master/artifactory-values.yaml)
* Updated Artifactory version to 7.12.5

## [11.7.1] - Dec 21, 2020
* Updated Artifactory version to 7.12.3

## [11.7.0] - Dec 18, 2020
* Updated Artifactory version to 7.12.2
* Added `.Values.artifactory.openMetrics.enabled`

## [11.6.1] - Dec 11, 2020
* Added configurable `.Values.global.versions.artifactory` in values.yaml

## [11.6.0] - Dec 10, 2020
* Update postgresql tag version to `12.5.0-debian-10-r25`
* Fixed `artifactory.persistence.googleStorage.endpoint` from `storage.googleapis.com` to `commondatastorage.googleapis.com`
* Updated chart maintainers email

## [11.5.5] - Dec 4, 2020
* **Important:** Renamed `.Values.systemYaml` to `.Values.systemYamlOverride`

## [11.5.4] - Dec 1, 2020
* Improve error message returned when attempting helm upgrade command

## [11.5.3] - Nov 30, 2020
* Updated Artifactory version to 7.11.5 - [Release Notes](https://www.jfrog.com/confluence/display/JFROG/Artifactory+Release+Notes#ArtifactoryReleaseNotes-Artifactory7.11)

## [11.5.2] - Nov 23, 2020
* Updated Artifactory version to 7.11.2 - [Release Notes](https://www.jfrog.com/confluence/display/JFROG/Artifactory+Release+Notes#ArtifactoryReleaseNotes-Artifactory7.11)
* Updated port namings on services and pods to allow for istio protocol discovery
* Change semverCompare checks to support hosted Kubernetes
* Add flag to disable creation of ServiceMonitor when enabling prometheus metrics
* Prevent the PostHook command to be executed if the user did not specify a command in the values file
* Fix issue with tls file generation when nginx.https.enabled is false

## [11.5.1] - Nov 19, 2020
* Updated Artifactory version to 7.11.2
* Bugfix - access.config.import.xml override Access Federation configurations

## [11.5.0] - Nov 17, 2020
* Updated Artifactory version to 7.11.1
* Update alpine tag version to `3.12.1`

## [11.4.6] - Nov 10, 2020
* Pass system.yaml via external secret for advanced usecases
* Added support for custom ingress
* Bugfix - stateful set not picking up changes to database secrets

## [11.4.5] - Nov 9, 2020
* Updated Artifactory version to 7.10.6 - [Release Notes](https://www.jfrog.com/confluence/display/JFROG/Artifactory+Release+Notes#ArtifactoryReleaseNotes-Artifactory7.10.6)

## [11.4.4] - Nov 2, 2020
* Add enablePathStyleAccess property for aws-s3-v3 binary provider template

## [11.4.3] - Nov 2, 2020
* Updated Artifactory version to 7.10.5 - [Release Notes](https://www.jfrog.com/confluence/display/JFROG/Artifactory+Release+Notes#ArtifactoryReleaseNotes-Artifactory7.10.5)

## [11.4.2] - Oct 22, 2020
* Chown bug fix where Linux capability cannot chown all files causing log line warnings
* Fix Frontend timeout linting issue

## [11.4.1] - Oct 20, 2020
* Add flag to disable prepare-custom-persistent-volume init container

## [11.4.0] - Oct 19, 2020
* Updated Artifactory version to 7.10.2 - [Release Notes](https://www.jfrog.com/confluence/display/JFROG/Artifactory+Release+Notes#ArtifactoryReleaseNotes-Artifactory7.10.2)

## [11.3.2] - Oct 15, 2020
* Add support to specify priorityClassName for nginx deployment

## [11.3.1] - Oct 9, 2020
* Add support for customInitContainersBegin

## [11.3.0] - Oct 7, 2020
* Updated Artifactory version to 7.9.1
* **Breaking change:** Fix `storageClass` to correct `storageClassName` in values.yaml

## [11.2.0] - Oct 5, 2020
* Expose Prometheus metrics via a ServiceMonitor
* Parse log files for metric data with Fluentd

## [11.1.0] - Sep 30, 2020
* Updated Artifactory version to 7.9.0 - [Release Notes](https://www.jfrog.com/confluence/display/JFROG/Artifactory+Release+Notes#ArtifactoryReleaseNotes-Artifactory7.9)
* Added support for resources in init container

## [11.0.11] - Sep 25, 2020
* Update to use linux capability CAP_CHOWN instead of root base init container to avoid any use of root containers to pass Redhat security requirements

## [11.0.10] - Sep 28, 2020
* Setting chart coordinates in migitation yaml

## [11.0.9] - Sep 25, 2020
* Update filebeat version to `7.9.2`

## [11.0.8] - Sep 24, 2020
* Fixed broken issue - when setting `waitForDatabase: false` container startup still waits for DB

## [11.0.7] - Sep 22, 2020
* Readme updates

## [11.0.6] - Sep 22, 2020
* Fix lint issue in migitation yaml

## [11.0.5] - Sep 22, 2020
* Fix broken migitation yaml

## [11.0.4] - Sep 21, 2020
* Added mitigation yaml for Artifactory - [More info](https://github.com/jfrog/chartcenter/blob/master/docs/securitymitigationspec.md)

## [11.0.3] - Sep 17, 2020
* Added configurable session(UI) timeout in frontend microservice

## [11.0.2] - Sep 17, 2020
* Added proper required text to be shown while postgres upgrades

## [11.0.1] - Sep 14, 2020
* Updated Artifactory version to 7.7.8 - [Release Notes](https://www.jfrog.com/confluence/display/JFROG/Artifactory+Release+Notes#ArtifactoryReleaseNotes-Artifactory7.7.8)

## [11.0.0] - Sep 2, 2020
* **Breaking change:** Changed `imagePullSecrets`values from string to list.
* **Breaking change:** Added `image.registry` and changed `image.version` to `image.tag` for docker images
* Added support for global values
* Updated maintainers in chart.yaml
* Update postgresql tag version to `12.3.0-debian-10-r71`
* Update postgresql chart version to `9.3.4` in requirements.yaml - [9.x Upgrade Notes](https://github.com/bitnami/charts/tree/master/bitnami/postgresql#900)
* **IMPORTANT**
* If this is a new deployment or you already use an external database (`postgresql.enabled=false`), these changes **do not affect you**!
* If this is an upgrade and you are using the default PostgreSQL (`postgresql.enabled=true`), you need to pass previous 9.x/10.x's postgresql.image.tag and databaseUpgradeReady=true

## [10.1.0] - Aug 13, 2020
* Updated Artifactory version to 7.7.3 - [Release Notes](https://www.jfrog.com/confluence/display/JFROG/Artifactory+Release+Notes#ArtifactoryReleaseNotes-Artifactory7.7)

## [10.0.15] - Aug 10, 2020
* Added enableSignedUrlRedirect for persistent storage type aws-s3-v3.

## [10.0.14] - Jul 31, 2020
* Update the README section on Nginx SSL termination to reflect the actual YAML structure.

## [10.0.13] - Jul 30, 2020
* Added condition to disable the migration scripts.

## [10.0.12] - Jul 28, 2020
* Document Artifactory node affinity.

## [10.0.11] - Jul 28, 2020
* Added maxConnections for persistent storage type aws-s3-v3.

## [10.0.10] - Jul 28, 2020
* Bugfix / support for userPluginSecrets with Artifactory 7

## [10.0.9] - Jul 27, 2020
* Add tpl to external database secrets
* Modified `scheme`  to `artifactory.scheme`

## [10.0.8] - Jul 23, 2020
* Added condition to disable the migration init container.

## [10.0.7] - Jul 21, 2020
* Updated Artifactory Chart to add node and primary labels to pods and service objects.

## [10.0.6] - Jul 20, 2020
* Support custom CA and certificates

## [10.0.5] - Jul 13, 2020
* Updated Artifactory version to 7.6.3 - https://www.jfrog.com/confluence/display/JFROG/Artifactory+Release+Notes#ArtifactoryReleaseNotes-Artifactory7.6.3
* Fixed Mysql database jar path in `preStartCommand` in README

## [10.0.4] - Jul 10, 2020
* Move some postgresql values to where they should be according to the subchart

## [10.0.3] - Jul 8, 2020
* Set Artifactory access client connections to the same value as the access threads

## [10.0.2] - Jul 6, 2020
* Updated Artifactory version to 7.6.2
* **IMPORTANT**
* Added ChartCenter Helm repository in README

## [10.0.1] - Jul 01, 2020
* Add dedicated ingress object for Replicator service when enabled

## [10.0.0] - Jun 30, 2020
* Update postgresql tag version to `10.13.0-debian-10-r38`
* Update alpine tag version to `3.12`
* Update busybox tag version to `1.31.1`
* **IMPORTANT**
* If this is a new deployment or you already use an external database (`postgresql.enabled=false`), these changes **do not affect you**!
* If this is an upgrade and you are using the default PostgreSQL (`postgresql.enabled=true`), you need to pass postgresql.image.tag=9.6.18-debian-10-r7 and databaseUpgradeReady=true

## [9.6.0] - Jun 29, 2020
* Updated Artifactory version to 7.6.1 - https://www.jfrog.com/confluence/display/JFROG/Artifactory+Release+Notes#ArtifactoryReleaseNotes-Artifactory7.6.1
* Add tpl for external database secrets

## [9.5.5] - Jun 25, 2020
* Stop loading the Nginx stream module because it is now a core module

## [9.5.4] - Jun 25, 2020
* Notes.txt update - add --namespace parameter

## [9.5.3] - Jun 11, 2020
* Support list of custom secrets

## [9.5.2] - Jun 12, 2020
* Updated Artifactory version to 7.5.7 - https://www.jfrog.com/confluence/display/JFROG/Artifactory+Release+Notes#ArtifactoryReleaseNotes-Artifactory7.5.7

## [9.5.1] - Jun 8, 2020
* Readme update - configuring Artifactory with oracledb

## [9.5.0] - Jun 1, 2020
* Updated Artifactory version to 7.5.5 - https://www.jfrog.com/confluence/display/JFROG/Artifactory+Release+Notes#ArtifactoryReleaseNotes-Artifactory7.5
* Fixes bootstrap configMap permission issue
* Update postgresql tag version to `9.6.18-debian-10-r7`

## [9.4.9] - May 27, 2020
* Added Tomcat maxThreads & acceptCount

## [9.4.8] - May 25, 2020
* Fixed postgresql README `image` Parameters

## [9.4.7] - May 24, 2020
* Fixed typo in README regarding migration timeout

## [9.4.6] - May 19, 2020
* Added metadata maxOpenConnections

## [9.4.5] - May 07, 2020
* Fix `installerInfo` string format

## [9.4.4] - Apr 27, 2020
* Updated Artifactory version to 7.4.3

## [9.4.3] - Apr 26, 2020
* Change order of the customInitContainers to run before the "migration-artifactory" initContainer.

## [9.4.2] - Apr 24, 2020
* Fix `artifactory.persistence.awsS3V3.useInstanceCredentials` incorrect conditional logic
* Bump postgresql tag version to `9.6.17-debian-10-r72` in values.yaml

## [9.4.1] - Apr 16, 2020
* Custom volumes in migration init container.

## [9.4.0] - Apr 14, 2020
* Updated Artifactory version to 7.4.1

## [9.3.1] - April 13, 2020
* Update README with helm v3 commands

## [9.3.0] - April 10, 2020
* Use dependency charts from `https://charts.bitnami.com/bitnami`
* Bump postgresql chart version to `8.7.3` in requirements.yaml
* Bump postgresql tag version to `9.6.17-debian-10-r21` in values.yaml

## [9.2.9] - Apr 8, 2020
* Added recommended ingress annotation to avoid 413 errors

## [9.2.8] - Apr 8, 2020
* Moved migration scripts under `files` directory
* Support preStartCommand in migration Init container as `artifactory.migration.preStartCommand`

## [9.2.7] - Apr 6, 2020
* Fix cache size (should be 5gb instead of 50gb since volume claim is only 20gb).

## [9.2.6] - Apr 1, 2020
* Support masterKey and joinKey as secrets

## [9.2.5] - Apr 1, 2020
* Fix readme use to `-hex 32` instead of `-hex 16`

## [9.2.4] - Mar 31, 2020
* Change the way the artifactory `command:` is set so it will properly pass a SIGTERM to java

## [9.2.3] - Mar 29, 2020
* Add Nginx log options: stderr as logfile and log level

## [9.2.2] - Mar 30, 2020
* Use the same defaulting mechanism used for the artifactory version used elsewhere in the chart

## [9.2.1] - Mar 29, 2020
* Fix loggers sidecars configurations to support new file system layout and new log names

## [9.2.0] - Mar 29, 2020
* Fix broken admin user bootstrap configuration
* **Breaking change:** renamed `artifactory.accessAdmin` to `artifactory.admin`

## [9.1.5] - Mar 26, 2020
* Fix volumeClaimTemplate issue

## [9.1.4] - Mar 25, 2020
* Fix volume name used by filebeat container

## [9.1.3] - Mar 24, 2020
* Use `postgresqlExtendedConf` for setting custom PostgreSQL configuration (instead of `postgresqlConfiguration`)

## [9.1.2] - Mar 22, 2020
* Support for SSL offload in Nginx service(LoadBalancer) layer. Introduced `nginx.service.ssloffload` field with boolean type.

## [9.1.1] - Mar 23, 2020
* Moved installer info to values.yaml so it is fully customizable

## [9.1.0] - Mar 23, 2020
* Updated Artifactory version to 7.3.2

## [9.0.29] - Mar 20, 2020
* Add support for masterKey trim during 6.x to 7.x migration if 6.x masterKey is 32 hex (64 characters)

## [9.0.28] - Mar 18, 2020
* Increased Nginx proxy_buffers size

## [9.0.27] - Mar 17, 2020
* Changed all single quotes to double quotes in values files
* useInstanceCredentials variable was declared in S3 settings but not used in chart. Now it is being used.

## [9.0.26] - Mar 17, 2020
* Fix rendering of Service Account annotations

## [9.0.25] - Mar 16, 2020
* Update Artifactory readme with extra ingress annotations needed for Artifactory to be set as SSO provider

## [9.0.24] - Mar 16, 2020
* Add Unsupported message from 6.18 to 7.2.x (migration)

## [9.0.23] - Mar 12, 2020
* Fix README.md rendering issue

## [9.0.22] - Mar 11, 2020
* Upgrade Docs update

## [9.0.21] - Mar 11, 2020
* Unified charts public release

## [9.0.20] - Mar 6, 2020
* Fix path to `/artifactory_bootstrap`
* Add support for controlling the name of the ingress and allow to set more than one cname

## [9.0.19] - Mar 4, 2020
* Add support for disabling `consoleLog` in `system.yaml` file

## [9.0.18] - Feb 28, 2020
* Add support to process `valueFrom` for extraEnvironmentVariables

## [9.0.17] - Feb 26, 2020
* Fix join key secret naming

## [9.0.16] - Feb 26, 2020
* Store join key to secret

## [9.0.15] - Feb 26, 2020
* Updated Artifactory version to 7.2.1

## [9.0.10] - Feb 07, 2020
* Remove protection flag `databaseUpgradeReady` which was added to check internal postgres upgrade

## [9.0.0] - Feb 07, 2020
* Updated Artifactory version to 7.0.0

## [8.4.8] - Feb 13, 2020
* Add support for SSH authentication to Artifactory

## [8.4.7] - Feb 11, 2020
* Change Artifactory service port name to be hard-coded to `http` instead of using `{{ .Release.Name }}`

## [8.4.6] - Feb 9, 2020
* Add support for `tpl` in the `postStartCommand`

## [8.4.5] - Feb 4, 2020
* Support customisable Nginx kind

## [8.4.4] - Feb 2, 2020
* Add a comment stating that it is recommended to use an external PostgreSQL with a static password for production installations

## [8.4.3] - Jan 30, 2020
* Add the option to configure resources for the logger containers

## [8.4.2] - Jan 26, 2020
* Improve `database.user` and `database.password` logic in order to support more use cases and make the configuration less repetitive

## [8.4.1] - Jan 19, 2020
* Fix replicator port config in nginx replicator configmap

## [8.4.0] - Jan 19, 2020
* Updated Artifactory version to 6.17.0

## [8.3.6] - Jan 16, 2020
* Added example for external nginx-ingress

## [8.3.5] - Dec 30, 2019
* Fix for nginx probes failing when launched with http disabled

## [8.3.4] - Dec 24, 2019
* Better support for custom `artifactory.internalPort`

## [8.3.3] - Dec 23, 2019
* Mark empty map values with `{}`

## [8.3.2] - Dec 16, 2019
* Fix for toggling nginx service ports

## [8.3.1] - Dec 12, 2019
* Add support for toggling nginx service ports

## [8.3.0] - Dec 1, 2019
* Updated Artifactory version to 6.16.0

## [8.2.6] - Nov 28, 2019
* Add support for using existing PriorityClass

## [8.2.5] - Nov 27, 2019
* Add support for PriorityClass

## [8.2.4] - Nov 21, 2019
* Add an option to use a file system cache-fs with the file-system binarystore template

## [8.2.3] - Nov 20, 2019
* Update Artifactory Readme

## [8.2.2] - Nov 20, 2019
* Update Artfactory logo

## [8.2.1] - Nov 18, 2019
* Add the option to provide service account annotations (in order to support stuff like https://docs.aws.amazon.com/eks/latest/userguide/specify-service-account-role.html)

## [8.2.0] - Nov 18, 2019
* Updated Artifactory version to 6.15.0

## [8.1.11] - Nov 17, 2019
* Do not provide a default master key. Allow it to be auto generated by Artifactory on first startup

## [8.1.10] - Nov 17, 2019
* Fix creation of double slash in nginx artifactory configuration

## [8.1.9] - Nov 14, 2019
* Set explicit `postgresql.postgresqlPassword=""` to avoid helm v3 error

## [8.1.8] - Nov 12, 2019
* Updated Artifactory version to 6.14.1

## [8.1.7] - Nov 9, 2019
* Additional documentation for masterKey

## [8.1.6] - Nov 10, 2019
* Update PostgreSQL chart version to 7.0.1
* Use formal PostgreSQL configuration format

## [8.1.5] - Nov 8, 2019
* Add support `artifactory.service.loadBalancerSourceRanges` for whitelisting when setting `artifactory.service.type=LoadBalancer`

## [8.1.4] - Nov 6, 2019
* Add support for any type of environment variable by using `extraEnvironmentVariables` as-is

## [8.1.3] - Nov 6, 2019
* Add nodeselector support for Postgresql

## [8.1.2] - Nov 5, 2019
* Add support for the aws-s3-v3 filestore, which adds support for pod IAM roles

## [8.1.1] - Nov 4, 2019
* When using `copyOnEveryStartup`, make sure that the target base directories are created before copying the files

## [8.1.0] - Nov 3, 2019
* Updated Artifactory version to 6.14.0

## [8.0.1] - Nov 3, 2019
* Make sure the artifactory pod exits when one of the pre-start stages fail

## [8.0.0] - Oct 27, 2019
**IMPORTANT - BREAKING CHANGES!**<br>
**DOWNTIME MIGHT BE REQUIRED FOR AN UPGRADE!**
* If this is a new deployment or you already use an external database (`postgresql.enabled=false`), these changes **do not affect you**!
* If this is an upgrade and you are using the default PostgreSQL (`postgresql.enabled=true`), must use the upgrade instructions in [UPGRADE_NOTES.md](UPGRADE_NOTES.md)!
* PostgreSQL sub chart was upgraded to version `6.5.x`. This version is **not backward compatible** with the old version (`0.9.5`)!
* Note the following **PostgreSQL** Helm chart changes
  * The chart configuration has changed! See [values.yaml](values.yaml) for the new keys used
  * **PostgreSQL** is deployed as a StatefulSet
  * See [PostgreSQL helm chart](https://hub.helm.sh/charts/stable/postgresql) for all available configurations

## [7.18.3] - Oct 24, 2019
* Change the preStartCommand to support templating

## [7.18.2] - Oct 21, 2019
* Add support for setting `artifactory.labels`
* Add support for setting `nginx.labels`

## [7.18.1] - Oct 10, 2019
* Updated Artifactory version to 6.13.1

## [7.18.0] - Oct 7, 2019
* Updated Artifactory version to 6.13.0

## [7.17.5] - Sep 24, 2019
* Option to skip wait-for-db init container with '--set waitForDatabase=false'

## [7.17.4] - Sep 11, 2019
* Updated Artifactory version to 6.12.2

## [7.17.3] - Sep 9, 2019
* Updated Artifactory version to 6.12.1

## [7.17.2] - Aug 22, 2019
* Fix the nginx server_name directive used with ingress.hosts

## [7.17.1] - Aug 21, 2019
* Enable the Artifactory container's liveness and readiness probes

## [7.17.0] - Aug 21, 2019
* Updated Artifactory version to 6.12.0

## [7.16.11] - Aug 14, 2019
* Updated Artifactory version to 6.11.6

## [7.16.10] - Aug 11, 2019
* Fix Ingress routing and add an example

## [7.16.9] - Aug 5, 2019
* Do not mount `access/etc/bootstrap.creds` unless user specifies a custom password or secret (Access already generates a random password if not provided one)
* If custom `bootstrap.creds` is provided (using keys or custom secret), prepare it with an init container so the temp file does not persist

## [7.16.8] - Aug 4, 2019
* Improve binarystore config
    1. Convert to a secret
    2. Move config to values.yaml
    3. Support an external secret

## [7.16.7] - Jul 29, 2019
* Don't create the nginx configmaps when nginx.enabled is false

## [7.16.6] - Jul 24, 2019
* Simplify nginx setup and shorten initial wait for probes

## [7.16.5] - Jul 22, 2019
* Change Ingress API to be compatible with recent kubernetes versions

## [7.16.4] - Jul 22, 2019
* Updated Artifactory version to 6.11.3

## [7.16.3] - Jul 11, 2019
* Add ingress.hosts to the Nginx server_name directive when ingress is enabled to help with Docker repository sub domain configuration

## [7.16.2] - Jul 3, 2019
* Fix values key in reverse proxy example

## [7.16.1] - Jul 1, 2019
* Updated Artifactory version to 6.11.1

## [7.16.0] - Jun 27, 2019
* Update Artifactory version to 6.11 and add restart to Artifactory when bootstrap.creds file has been modified

## [7.15.8] - Jun 27, 2019
* Add the option for changing nginx config using values.yaml and remove outdated reverse proxy documentation

## [7.15.6] - Jun 24, 2019
* Update chart maintainers

## [7.15.5] - Jun 24, 2019
* Change Nginx to point to the artifactory externalPort

## [7.15.4] - Jun 23, 2019
* Add the option to provide an IP for the access-admin endpoints

## [7.15.3] - Jun 23, 2019
* Add values files for small, medium and large installations

## [7.15.2] - Jun 20, 2019
* Add missing terminationGracePeriodSeconds to values.yaml

## [7.15.1] - Jun 19, 2019
* Updated Artifactory version to 6.10.4

## [7.15.0] - Jun 17, 2019
* Use configmaps for nginx configuration and remove nginx postStart command

## [7.14.8] - Jun 18, 2019
* Add the option to provide additional ingress rules

## [7.14.7] - Jun 14, 2019
* Updated readme with improved external database setup example

## [7.14.6] - Jun 11, 2019
* Updated Artifactory version to 6.10.3
* Updated installer-info template

## [7.14.5] - Jun 6, 2019
* Updated Google Cloud Storage API URL and https settings

## [7.14.4] - Jun 5, 2019
* Delete the db.properties file on Artifactory startup

## [7.14.3] - Jun 3, 2019
* Updated Artifactory version to 6.10.2

## [7.14.2] - May 21, 2019
* Updated Artifactory version to 6.10.1

## [7.14.1] - May 19, 2019
* Fix missing logger image tag

## [7.14.0] - May 7, 2019
* Updated Artifactory version to 6.10.0

## [7.13.21] - May 5, 2019
* Add support for setting `artifactory.async.corePoolSize`

## [7.13.20] - May 2, 2019
* Remove unused property `artifactory.releasebundle.feature.enabled`

## [7.13.19] - May 1, 2019
* Fix indentation issue with the replicator system property

## [7.13.18] - Apr 30, 2019
* Add support for JMX monitoring

## [7.13.17] - Apr 25, 2019
* Added support for `cacheProviderDir`

## [7.13.16] - Apr 18, 2019
* Changing API StatefulSet version to `v1` and permission fix for custom `artifactory.conf` for Nginx

## [7.13.15] - Apr 16, 2019
* Updated documentation for Reverse Proxy Configuration

## [7.13.14] - Apr 15, 2019
* Added support for `customVolumeMounts`

## [7.13.13] - Aprl 12, 2019
* Added support for `bucketExists` flag for googleStorage

## [7.13.12] - Apr 11, 2019
* Replace `curl` examples with `wget` due to the new base image

## [7.13.11] - Aprl 07, 2019
* Add support for providing the Artifactory license as a parameter

## [7.13.10] - Apr 10, 2019
* Updated Artifactory version to 6.9.1

## [7.13.9] - Aprl 04, 2019
* Add support for templated extraEnvironmentVariables

## [7.13.8] - Aprl 07, 2019
* Change network policy API group

## [7.13.7] - Aprl 04, 2019
* Bugfix for userPluginSecrets

## [7.13.6] - Apr 4, 2019
* Add information about upgrading Artifactory with auto-generated postgres password

## [7.13.5] - Aprl 03, 2019
* Added installer info

## [7.13.4] - Aprl 03, 2019
* Allow secret names for user plugins to contain template language

## [7.13.3] - Apr 02, 2019
* Allow NetworkPolicy configurations (defaults to allow all)

## [7.13.2] - Aprl 01, 2019
* Add support for user plugin secret

## [7.13.1] - Mar 27, 2019
* Add the option to copy a list of files to ARTIFACTORY_HOME on startup

## [7.13.0] - Mar 26, 2019
* Updated Artifactory version to 6.9.0

## [7.12.18] - Mar 25, 2019
* Add CI tests for persistence, ingress support and nginx

## [7.12.17] - Mar 22, 2019
* Add the option to change the default access-admin password

## [7.12.16] - Mar 22, 2019
* Added support for `<artifactory|nginx>.<readiness|liveness>Probe.path` to customise the paths used for health probes

## [7.12.15] - Mar 21, 2019
* Added support for `artifactory.customSidecarContainers` to create custom sidecar containers
* Added support for `artifactory.customVolumes` to create custom volumes

## [7.12.14] - Mar 21, 2019
* Make ingress path configurable

## [7.12.13] - Mar 19, 2019
* Move the copy of bootstrap config from postStart to preStart

## [7.12.12] - Mar 19, 2019
* Fix existingClaim example

## [7.12.11] - Mar 18, 2019
* Add information about nginx persistence

## [7.12.10] - Mar 15, 2019
* Wait for nginx configuration file before using it

## [7.12.9] - Mar 15, 2019
* Revert securityContext changes since they were causing issues

## [7.12.8] - Mar 15, 2019
* Fix issue #247 (init container failing to run)

## [7.12.7] - Mar 14, 2019
* Updated Artifactory version to 6.8.7
* Add support for Artifactory-CE for C++

## [7.12.6] - Mar 13, 2019
* Move securityContext to container level

## [7.12.5] - Mar 11, 2019
* Updated Artifactory version to 6.8.6

## [7.12.4] - Mar 8, 2019
* Fix existingClaim option

## [7.12.3] - Mar 5, 2019
* Updated Artifactory version to 6.8.4

## [7.12.2] - Mar 4, 2019
* Add support for catalina logs sidecars

## [7.12.1] - Feb 27, 2019
* Updated Artifactory version to 6.8.3

## [7.12.0] - Feb 25, 2019
* Add nginx support for tail sidecars

## [7.11.1] - Feb 20, 2019
* Added support for enterprise storage

## [7.10.2] - Feb 19, 2019
* Updated Artifactory version to 6.8.2

## [7.10.1] - Feb 17, 2019
* Updated Artifactory version to 6.8.1
* Add example of `SERVER_XML_EXTRA_CONNECTOR` usage

## [7.10.0] - Feb 15, 2019
* Updated Artifactory version to 6.8.0

## [7.9.6] - Feb 13, 2019
* Updated Artifactory version to 6.7.3

## [7.9.5] - Feb 12, 2019
*  Add support for tail sidecars to view logs from k8s api

## [7.9.4] - Feb 6, 2019
* Fix support for customizing statefulset `terminationGracePeriodSeconds`

## [7.9.3] - Feb 5, 2019
* Add instructions on how to deploy Artifactory with embedded Derby database

## [7.9.2] - Feb 5, 2019
* Add support for customizing statefulset `terminationGracePeriodSeconds`

## [7.9.1] - Feb 3, 2019
* Updated Artifactory version to 6.7.2

## [7.9.0] - Jan 23, 2019
* Updated Artifactory version to 6.7.0

## [7.8.9] - Jan 22, 2019
* Added support for `artifactory.customInitContainers` to create custom init containers

## [7.8.8] - Jan 17, 2019
* Added support of values ingress.labels

## [7.8.7] - Jan 16, 2019
* Mount replicator.yaml (config) directly to /replicator_extra_conf

## [7.8.6] - Jan 13, 2019
* Fix documentation about nginx group id

## [7.8.5] - Jan 13, 2019
* Updated Artifactory version to 6.6.5

## [7.8.4] - Jan 8, 2019
* Make artifactory.replicator.publicUrl required when the replicator is enabled

## [7.8.3] - Jan 1, 2019
* Updated Artifactory version to 6.6.3
* Add support for `artifactory.extraEnvironmentVariables` to pass more environment variables to Artifactory

## [7.8.2] - Dec 28, 2018
* Fix location `replicator.yaml` is copied to

## [7.8.1] - Dec 27, 2018
* Updated Artifactory version to 6.6.1

## [7.8.0] - Dec 20, 2018
* Updated Artifactory version to 6.6.0

## [7.7.13] - Dec 17, 2018
* Updated Artifactory version to 6.5.13

## [7.7.12] - Dec 12, 2018
* Fix documentation about Artifactory license setup using secret

## [7.7.11] - Dec 10, 2018
* Fix issue when using existing claim

## [7.7.10] - Dec 5, 2018
* Remove Distribution certificates creation.

## [7.7.9] - Nov 30, 2018
* Updated Artifactory version to 6.5.9

## [7.7.8] - Nov 29, 2018
* Updated postgresql version to 9.6.11

## [7.7.7] - Nov 27, 2018
* Updated Artifactory version to 6.5.8

## [7.7.6] - Nov 19, 2018
* Added support for configMap to use custom Reverse Proxy Configuration with Nginx

## [7.7.5] - Nov 14, 2018
* Fix location of `nodeSelector`, `affinity` and `tolerations`

## [7.7.4] - Nov 14, 2018
* Updated Artifactory version to 6.5.3

## [7.7.3] - Nov 12, 2018
* Support artifactory.preStartCommand for running command before entrypoint starts

## [7.7.2] - Nov 7, 2018
* Support database.url parameter (DB_URL)

## [7.7.1] - Oct 29, 2018
* Change probes port to 8040 (so they will not be blocked when all tomcat threads on 8081 are exhausted)

## [7.7.0] - Oct 28, 2018
* Update postgresql chart to version 0.9.5 to be able and use `postgresConfig` options

## [7.6.8] - Oct 23, 2018
* Fix providing external secret for database credentials

## [7.6.7] - Oct 23, 2018
* Allow user to configure externalTrafficPolicy for Loadbalancer

## [7.6.6] - Oct 22, 2018
* Updated ingress annotation support (with examples) to support docker registry v2

## [7.6.5] - Oct 21, 2018
* Updated Artifactory version to 6.5.2

## [7.6.4] - Oct 19, 2018
* Allow providing pre-existing secret containing master key
* Allow arbitrary annotations on primary and member node pods
* Enforce size limits when using local storage with `emptyDir`
* Allow providing pre-existing secrets containing external database credentials

## [7.6.3] - Oct 18, 2018
* Updated Artifactory version to 6.5.1

## [7.6.2] - Oct 17, 2018
* Add Apache 2.0 license

## [7.6.1] - Oct 11, 2018
* Supports master-key in the secrets and stateful-set
* Allows ingress default `backend` to be enabled or disabled (defaults to enabled)

## [7.6.0] - Oct 11, 2018
* Updated Artifactory version to 6.5.0

## [7.5.4] - Oct 9, 2018
* Quote ingress hosts to support wildcard names

## [7.5.3] - Oct 4, 2018
* Add PostgreSQL resources template

## [7.5.2] - Oct 2, 2018
* Add `helm repo add jfrog https://charts.jfrog.io` to README

## [7.5.1] - Oct 2, 2018
* Set Artifactory to 6.4.1

## [7.5.0] - Sep 27, 2018
* Set Artifactory to 6.4.0

## [7.4.3] - Sep 26, 2018
* Add ci/test-values.yaml

## [7.4.2] - Sep 2, 2018
* Updated Artifactory version to 6.3.2
* Removed unused PVC

## [7.4.0] - Aug 22, 2018
* Added support to run as non root
* Updated Artifactory version to 6.2.0

## [7.3.0] - Aug 22, 2018
* Enabled RBAC Support
* Added support for PostStartCommand (To download Database JDBC connector)
* Increased postgresql max_connections
* Added support for `nginx.conf` ConfigMap
* Updated Artifactory version to 6.1.0
