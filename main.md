# ISDT Inventories Description

## Internal Files

A large part of the inventories is generated using playbooks. This includes files such as `hosts`, `build-releases-*`, `stack-*`, and the lists of components in the `host_vars` files. These generated files are managed by internal files.

```mermaid
graph TD;
  subgraph Internals_files
    style Internals_files fill:transparent, stroke:#ffcccc, stroke-width:3px, stroke-dasharray: 0;
    
    style A fill:#000, stroke:#f9f, stroke-width:2px, stroke-dasharray: 0, color:#fff;
    style B fill:#000, stroke:#9f9, stroke-width:2px, stroke-dasharray: 0, color:#fff;
    style C fill:#000, stroke:#ff9, stroke-width:2px, stroke-dasharray: 0, color:#fff;
    style D fill:#000, stroke:#9ff, stroke-width:2px, stroke-dasharray: 0, color:#fff;
    style E fill:#000, stroke:#f99, stroke-width:2px, stroke-dasharray: 0, color:#fff;
  
    A["**Internal_infrastructure**<br/><hr style='margin-top: 8px; margin-bottom: 8px;'/>Define environments, instances <br/>hosts and host configuration"] -->|Release to use| B["**Internal_release**<br/><hr style='margin-top: 8px; margin-bottom: 8px;'/>Define components version <br/>by release"];
    B -->|components version| D["**Internal_stacks**<br/><hr style='margin-top: 8px; margin-bottom: 8px;'/>Define groups of components <br/>and version by group"];
    D -->|stacks| E["**Internal_platform**<br/><hr style='margin-top: 8px; margin-bottom: 8px;'/>Define components and release <br/>to use for each environment"];
    E -->|Release to use| B;
    C["**Internal_components**<br/><hr style='margin-top: 8px; margin-bottom: 8px;'/>Define available<br/> components"] -->|components| D;
  end;
```

- **Internal_infrastructure**: Defines the structure of environments (app / ISDT), instances (sbx, dev, devint, qa, prod), and worker nodes.
- **Internal_components**: Defines the list of all available components.
- **Internal_stacks**: Defines the stacks and the components for each stack.
- **Internal_platform**: Defines all platform environments (apps or ISDT), the stacks, and the release used by each environment.
- **Internal_release**: Defines the versions of all infrastructure and platform components.

## Internal Playbooks

After a change in internal files, the inventories should be regenerated.

### Generate Hosts, Build-Releases-*, and Stack-*

After any changes in internal files, this playbook should be executed:

```bash
ansible-playbook -i ./inventories playbooks/internal_Generate-Hosts.yml
```

### Generate Component List

After any changes in the components list, stacks, or components for an environment, this playbook should be executed:

```bash
ansible-playbook -i ./inventories playbooks/internal_Generate-Host-Components.yml
```

## Inventories Files



## Process How to Add a Component

### Add a New Standalone Component

TODO: Approximately the same steps as the following.

### Add a Component Dependent on Another

In this example, will see how I added the elasticsearch exporter, which is dependent on the already existing Prometheus component, to the repository.

For my exporter, I have a Helm chart in Github, and the container image required is also there.

Here are the steps to do:

1. Update `inventories/group_vars/internal_components.yml`
2. Update `inventories/group_vars/internal_releases.yml`
3. Update `inventories/group_vars/internal_stacks.yml`
4. Update `inventories/group_vars/internal_platform.yml`
5. Update the `inventories/group_vars/component_<component_name>.yml`
6. Add values file for your component in `isdt/playbooks/templates/Release_2.0/`
7. Run playbooks to generate proper files for deployment
8. Run playbook to deploy your component

#### Update inventories/group_vars/internal_components.yml

Add your component in the `components` array of `internal_components.yml`:

```yaml
components:
  # ... other components
  PROMETHEUS_ELASTICSEARCH_EXPORTER: # Choose a proper name for your component
    alias: PROMETHEUS # Use the name of the component that you depend on.
    image: none
    build: false
    is_Helm_Deployment: true
    versions:
      PROMETHEUS_ELASTICSEARCH_EXPORTER_581: # 581 corresponds to version name 5.8.1 of my exporter.
        name: PROMETHEUS_ELASTICSEARCH_EXPORTER
        label: PROMETHEUS_ELASTICSEARCH_EXPORTER_581
        description: "Elasticsearch exporter 5.8.1"
  # ... other components
```

#### Update inventories/group_vars/internal_releases.yml

Add your component to the components list:

```yaml
versions:
  - release: '3.5'
    component:
      # ... other components
      - name: PROMETHEUS_ELASTICSEARCH_EXPORTER_581
        helm: 5.8.1
        image: none
        app_version: 4.0.2
```

#### Update inventories/group_vars/internal_stacks.yml

Create a new pack in the ISDT section if it does not already exist, otherwise add your component to the corresponding existing pack:

```yaml
stacks:
  # ... other stacks
  - name: "ISDT"
    ws_service_type: "none"
    in_migration: "false"
    description: "ISDT Platform Components"
    packs:
      Prometheus:
        - PROMETHEUS
        - PROMETHEUS_ELASTICSEARCH_EXPORTER
      RookCeph:
        - ROOK_CEPH_OPERATOR 
        - ROOK_CEPH_CLUSTER
      ElasticStack:
        - ELASTIC_ELASTICSEARCH 
        - ELASTIC_KIBANA
    component:
      # ... other components
      - '{{ components.PROMETHEUS.versions.PROMETHEUS_060 }}'
      - '{{ components.PROMETHEUS_ELASTICSEARCH_EXPORTER.versions.PROMETHEUS_ELASTICSEARCH_EXPORTER_581 }}'
      - '{{ components.ROOK_CEPH_CLUSTER.versions.ROOK_CEPH_CLUSTER_110 }}'
      # ... other components
```

#### Update inventories/group_vars/internal_platform.yml

Modify the existing element to use the component pack defined earlier, and delete the `optional_component` line:

```yaml
internal_platform:
  environments:
    # ... other environments
    - name: sbx
      instances:
        - name: isdt
          release: "3.5"
          products:
            - name: isdt
              hosts:
                # ... other components
                - name: isdt-sbxprometheus
                  stack: "ISDT"
                  release: "3.5"
                  status: ":heavy_check_mark:"
                  component_pack: [Prometheus] # Use the exact same name of the pack defined in the previous step here
                  # optional_component: [PROMETHEUS] # DELETED
```

#### Update inventories/group_vars/component_prometheus.yml

Add Helm repository and Helm name for your component in the proper `component_prometheus.yml` file. As my component depends on Prometheus, I have to modify `component_prometheus.yml` instead of creating a new dedicated file. The lines must be prefixed by the lowercased name of the component.

```yaml
# Helm Base Configuration
prometheus_helm_repository: "https://prometheus-community.github.io/helm-charts"
prometheus_helm_name: "kube-prometheus-stack"
prom_host: 'monitoring.{{ host_domain }}'

prometheus_elasticsearch_exporter_helm_repository: "{{ prometheus_helm_repository }}"
prometheus_elasticsearch_exporter_helm_name: "prometheus-elasticsearch-exporter"
```

#### Add Values Files for Your Component: `isdt/playbooks/templates/Release_3.5/PROMETHEUS_ELASTICSEARCH_EXPORTER.yml`

Create the file and paste the YAML values for your component's Helm chart. Here, I'm using the same values as the default ones. As I created the Helm chart myself, I can simply copy my values file.

`isdt/playbooks/templates/Release_3.5/PROMETHEUS_ELASTICSEARCH_EXPORTER.yml`:

```yaml
prometheus-elasticsearch-exporter:
  es:
    uri: "http://user:password@elasticsearch-master.isdt-sbxelastic.svc.cluster.local:9200"
    all: true
    indices: true
    indices_settings: true
    indices_mappings: true
    shards: true
    snapshots: true
    timeout: 30s

  service:
    enabled: true
    type: ClusterIP
    httpPort: 9108

  replicas:
    count: 1

  securityContext:
    allowPrivilegeEscalation: false
    capabilities:
      drop:
        - ALL
    readOnlyRootFilesystem: true

  log:
    format: log

fmt
    level: info

  commandArgs:
    - "--web.listen-address=:9108"
    - "--web.telemetry-path=/metrics"
```

#### Run Playbooks to Generate Proper Files for Deployment

```bash
dzdo su - docker # Docker user is mandatory, otherwise you will encounter permission issues
cd /path/to/your/repository/platform-isdt/isdt/ # cd into your repository in your workspace
# Run the playbooks
ansible-playbook -i ./inventories/ playbooks/internal_Generate-Hosts.yml
ansible-playbook -i ./inventories/ playbooks/internal_Generate-Host-Components.yml
```

This will generate the proper files into your repository, which you can commit and push after testing the deployment of your component. Your component is then ready to deploy.

### Run Playbook to Deploy Your Component

Run the following command (replace `isdt-sbxprometheus` with the proper name for your component):

```bash
ansible-playbook -i inventories/ playbooks/platform_Generic-Deploy.yml -l isdt-sbx{{name_of_your_component}}
```

You may encounter a YAML parsing error:

```yaml
TASK [platform_Generic-Deploy : Helm Load Dependencies] **********************************************************************************************************************************************
fatal: [isdt-sbxprometheus]: FAILED! => {"changed": true, "cmd": ["helm", "dependency", "build"], "delta": "0:00:00.026715", "end": "2023-01-11 15:02:56.256125", "msg": "non-zero return code", "rc": 1, "start": "2023-01-11 15:02:56.229410", "stderr": "Error: cannot load values.yaml: error converting YAML to JSON: yaml: line 254: did not find expected key", "stderr_lines": ["Error: cannot load values.yaml: error converting YAML to JSON: yaml: line 254: did not find expected key"], "stdout": "", "stdout_lines": []}
```

The YAML generated at `/app/docker/devops/ISDT/deploy/workspace_temp/isdt-sbxprometheus/values.yaml` and used for the task "Helm load dependencies" was not valid in my case. That's because of an incorrect append between the values files of my components `platform-isdt/isdt/playbooks/templates/Release_3.5/PROMETHEUS_ELASTICSEARCH_EXPORTER.yml` and `platform-isdtisdt/playbooks/templates/Release_3.5/PROMETHEUS_ELASTICSEARCH_EXPORTER.yml`.

I could fix it by adding a newline at line 1 of my `PROMETHEUS_ELASTICSEARCH_EXPORTER.yml` file:

```diff
+ 
prometheus-elasticsearch-exporter:

  es:
    uri: "http://user:password@elasticsearch-master.isdt-sbxelastic.svc.cluster.local:9200"
    all: true
    indices: true
    indices_settings: true
    indices_mappings: true
    shards: true
    snapshots: true
    timeout: 30s

  service:
    enabled: true
    type: ClusterIP
    httpPort: 9108
```
```
