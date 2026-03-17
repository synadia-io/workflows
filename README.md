# Reusable GitHub Action Workflows

### Reference
GitHub Docs [reuse-workflows](https://docs.github.com/en/actions/how-tos/reuse-automations/reuse-workflows)

## Composite Actions

### Installing Nats Server
Reusable composite actions for installing and shutting down nats-server.

#### Install
Builds and installs nats-server from source. Optionally specify a git tag.

```yaml
# Latest (head of main)
- name: Install Nats Server
  uses: synadia-io/workflows/.github/actions/install_nats_server@main

# Specific tag
- name: Install Nats Server
  uses: synadia-io/workflows/.github/actions/install_nats_server@main
  with:
    tag: v2.10.24
```

* | Inputs | Description                                              | Required | Default        |
  |--------|----------------------------------------------------------|----------|----------------|
  | tag    | nats-server git tag to build (e.g. v2.10.24)             | no       | latest on main |

#### Shutdown
Kills any running nats-server processes.

```yaml
- name: Shutdown Nats Servers
  if: always()
  run: pkill -9 nats-server 2>/dev/null || true
```

## Java Workflows

### Standard
* JDK 21 Temurin
* Gradle 8+,
* head nats-server
* Supports subprojects
* Example
  ```yaml
  jobs:
    build:
      uses: synadia-io/workflows/.github/workflows/<workflow>.yml@main 
      with:
        project-dir: <project-dir>
        coverage: true
      secrets: inherit
  ```

#### Workflows
* PR: [java-standard-pr](.github/workflows/java-standard-pr.yml)
* Main: [java-standard-main](.github/workflows/java-standard-main.yml)
* Release: [java-standard-release](.github/workflows/java-standard-release.yml)
* | Inputs      | Description                          | Required | Default | PR | Main | Release |
  |-------------|--------------------------------------|----------|---------|:--:|:----:|:-------:|
  | project-dir | path for a project in a subdirectory | no       | ignored | ✓  |  ✓   |    ✓    |
  | coverage    | true if should run coverage step     | no       | false   | ✓  |  ✓   |         |

* | Behavior                         | PR | Main | Release |
  |----------------------------------|:--:|:----:|:-------:|
  | Test                             | ✓  |  ✓   |    ✓    |
  | Verify Javadoc                   | ✓  |  ✓   |    ✓    |
  | Coverage (optional)              | ✓  |  ✓   |         |
  | Publish Snapshot                 |    |  ✓   |         |
  | Verify, Sign and Publish Release |    |      |    ✓    |
