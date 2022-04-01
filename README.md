![image alt text](logo.png)

# North Data Documentation Hub

Please note that an API key is required for all our APIs.

- [Release Notes](https://github.com/northdata/api/releases) 
- [All Countries and Sources](https://www.northdata.com/_coverage)

## Data API 

The Data API allows you to access North Data's company database via https in JSON or XML format.

- [Data API User Guide](https://github.com/northdata/api/blob/master/doc/data-api-userguide/data-api-userguide.md)  
- [Data API Reference Guide](https://northdata.github.io/doc/api/) 
(generated from OpenAPI/Swagger 2.0 specification) 
- [OpenAPI/Swagger 2.0 YAML file](swagger.yaml) 
- [OpenAPI 3.0 YAML file](https://northdata.github.io/doc/api/openapi.yaml) (generated from OpenAPI/Swagger 2.0 specification) 
 
## Widget API 

The Widget API allows you to use North Data's interactive visualizations in your website or service, using simple Javascript calls. 

- [Widget API User Guide](https://github.com/northdata/api/blob/master/doc/widgetapi-userguide/widgetapi-userguide.md) (German)
      
## Performance indicators

- [Reference](https://www.northdata.com/_financials) 
        
## Support
      
Email <a href="mailto:support@northdata.com">support@northdata.com</a>

## Pushing a release to Maven Central

* Replace SNAPSHOT version in `pom.xml` by release version, e.g. `1.2.5`.
* `git commit`
* `git tag 1.2.5`
* `git push --tags`
* Wait for GitHub Actions to finish. The new version will appear on Maven Central after ~15 minutes.
* Replace version in `pom.xml` by next SNAPSHOT version, e.g. `1.2.6-SNAPSHOT`.
* `git commit`
* `git push`

