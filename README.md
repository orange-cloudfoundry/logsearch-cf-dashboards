# BOSH Release for Logsearch-cf-dashboards

This release is an add-on for [Logsearch] (http://www.logsearch.io) and its [BOSH release] (https://github.com/logsearch/logsearch-boshrelease) which offers import and export of dashboards, visualizations and searches in Elasticsearch under the index `.kibana`.

## Prerequisite

The release has been tested with
- Logsearch v203.0.0 (Kibana v4.4) and
- [Docker BOSH release] (https://github.com/cloudfoundry-community/docker-boshrelease) v27.
- [Spiff] (https://github.com/cloudfoundry-incubator/spiff)
- Internet access

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

The default dashboards, visualization and searches are now uploaded to your Logsearch (Elasticsearch). You can check them out.

### Export Dashboards - Visualization - Searches

To export dashboards, visualization and searches from your Logsearch (Elasticsearch), execute as follows:

```
bosh run errand export
```

The exported dashboards, visualization and searches from your Logsearch (Elasticsearch) are now uploaded to the instance of the deployment (`elasticdump/0`) under `/var/vcap/store/dashboards/kibana-exported.X.json` where `X` is an epoch value. All this is stored on the persistent disk attached to the instance.

## Development

Inspired by the project [elasticdump] (https://github.com/taskrabbit/elasticsearch-dump) we created a BOSH release to import and export dashboards, visualization and searches for the Cloud Foundry metrics inside Logsearch (Kibana). The release includes 4 jobs: import and export errand jobs, IP address filter job and a job to upload default dashboards, visualization and searches. The last 2 jobs are located on a single instance. This instance contains a Docker server which executes elasticdump to import and export.

### Import and Export Errand Jobs

These jobs represent 2 errands to import and export dashboards, visualization and searches. Each job includes elasticdump and a Docker client which sends commands to the Docker server on the main instance. The jobs take as variables the Docker server IP address and port (main instance) and IP address and port of Elasticsearch.

### IP Address Filter job

This is job aims to filter incoming requests to the Docker server on the main instance with iptables since the server is bind to `0.0.0.0`. So we limited access to the server to all the docker requests except from the import and export instances.




































