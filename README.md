# BOSH Release for Logsearch-cf-dashboards

This release is an add-on for [Logsearch] (http://www.logsearch.io) and its [BOSH release] (https://github.com/logsearch/logsearch-boshrelease) which offers import and export of dashboards, visualizations and searches in Elasticsearch under the index `.kibana`.

## Prerequisite

The release has been tested with
- Logsearch v203.0.0 (Kibana v4.4) and
- [Docker BOSH release] (https://github.com/cloudfoundry-community/docker-boshrelease) v27.

## Usage

To use this bosh release, first upload it to your bosh:

```
bosh target BOSH_HOST
git clone https://github.com/cloudfoundry-community/logsearch-cf-dashboards-boshrelease.git
cd logsearch-cf-dashboards-boshrelease
bosh create release --force
bosh upload release
```

### Deployment Manifests and Deploy

To create your stub manifest you can use our openstack example under examples/stub.yml.

For Openstack you can quickly create a deployment manifest & deploy:

```
./scripts/generate_deployment_manifest openstack FULL_PATH_TO_YOUR_STUB_MANIFEST > FULL_PATH_TO_FINAL_MANIFEST
bosh deployment FULL_PATH_TO_FINAL_MANIFEST
bosh -n deploy
```

### Import Dashboards - Visualization - Searches

To import default dashboards, visualization and searches, execute as follows:

```
bosh run errand import
```

### Export Dashboards - Visualization - Searches

To export dashboards, visualization and searches from your Logsearch (Elasticsearch), execute as follows:

```
bosh run errand export
```
