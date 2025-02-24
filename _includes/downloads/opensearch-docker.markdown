The best way to try out OpenSearch is to use [Docker Compose](https://docs.docker.com/compose/install/). These steps will set up a two node cluster of OpenSearch plus OpenSearch Dashboards:

1. Set up your Docker host environment 
  - **macOS & Windows**: In Docker _Preferences_ > _Resources_, set RAM to at least 4 GB.
  - **Linux**: Ensure `vm.max_map_count` is set to at least 262144 as per the [documentation](/docs/opensearch/install/important-settings/).
2. Download [docker-compose.yml](/samples/docker-compose.yml) into your desired directory
  - **Note for OpenSearch 2.12 or later:**
    Fresh installs of 2.12 or later require that you define an admin password --- using the `OPENSEARCH_INITIAL_ADMIN_PASSWORD` environment variable --- when configuring the security demo. For more information, see [Setting up a demo configuration](https://opensearch.org/docs/latest/security/configuration/demo-configuration/).
3. Run `docker compose up`
4. Have a nice coffee while everything is downloading and starting up
5. Navigate to [http://localhost:5601/](http://localhost:5601) for OpenSearch Dashboards
6. Login with the default username (`admin`) and password (`<custom-admin-password>`)
