## Pushing a release to Maven Central

* Replace SNAPSHOT version in `pom.xml` by release version, e.g. `1.2.5`.
* `git commit`
* `git tag 1.2.5`
* `git push --tags`
* Wait for GitHub Actions to finish. The new version will appear on Maven Central after ~15 minutes.
* Replace version in `pom.xml` by next SNAPSHOT version, e.g. `1.2.6-SNAPSHOT`.
* `git commit`
* `git push`

