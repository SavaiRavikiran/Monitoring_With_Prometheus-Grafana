```mermaid
graph TD;

  A["Global definition"] --> B["Instance X"];
  A --> C["Environment X"];
  A --> D["Build X"];
  A --> E["Product X"];
  
  // Second row
  B --> F["Platform X"];
  C --> G["Infra Y"];
  D --> H["Stacks X"];
  
  // Third row
  G --> I["Host Y"];
  H --> J["Component X"];
  J --> I["Host X"];
  F --> I["Host X"];
  E --> I["Host X"];

```

## Infrastructure Diagram

### Using Playbooks

* Define environments, instances, hosts, and host configuration (hosts, build-releases-, stack-, and the lists of components files)
* Define component version by release
* Define all available components
* Define groups of components and versions by group (stacks)
* Define components and release to use for each environment (internal_platform)

### Structure of environments (apps/isdt), instances (sbx, dev, devint, qa, prod), and worker nodes

* Define the infrastructure of all available components
* Define the components for each stacks
* Define the environments (apps or isdt), the stacks, and the release used by each environment
* Define all infrastructure and platform components

### batek.net

[Image of diagram]
