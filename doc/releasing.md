## Pushing a release to Maven Central

* Run the **Release to Maven Central** workflow from GitHub Actions.
* Enter the version tag parameter.
* The job will replace the default version 0.0.0-SNAPSHOT in `pom.xml` and `swagger.yaml`.
* If the job does not complete successfully, pull the branch and revert the automatic commit, if needed.

