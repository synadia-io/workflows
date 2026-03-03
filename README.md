# Reusable GitHub Action Workflows

### Reference
GitHub Docs [reuse-workflows](https://docs.github.com/en/actions/how-tos/reuse-automations/reuse-workflows)

## Java

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
