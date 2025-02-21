# api-designer-poc
A React application used as the upstream driver for Phase 1 of our API 
Designer project.  This UI provides a kind of mimic for the console.redhat.com
interface, allowing local development and testing in a way that approximates
the look/experience that will be available there.

# Build and run (local)
To run the app locally, do the following:

```bash
$ npm install
$ npm run start:dev
```

Then open your browser (if it doesn't automatically open) to:

http://localhost:8080/

Note that for the UI to work, you will also need to be running the "ads-ui"
UI, which provides a set of federated module components that this project uses.
The UI will fail to load if those are not available.  To run "ads-ui" go 
here:

https://github.com/bf2fc6cc711aee1a0c2a/ads-ui

The `ads-ui` UI should be available on port 9009 (which is the default port
configured for that project).

By default, `api-designer-poc` will start on localhost with authentication 
disabled.  This is called the "none" profile.  However, there are multiple
profiles supported in dev/local mode:

* `none` - the default mode with authentication disabled and local URLs for SRS and AMS APIs
* `staging` - authentication enabled (via Red Hat SSO) and using staging URLs for SRS and AMS APIs
* `prod` - authentication enabled (via Red Hat SSO) and using production URLs for SRS and AMS APIs

To run with an alternative profile, set the `API_DESIGNER_POC_CONFIG` environment
variable.  So for example:

```bash
$ export API_DESIGNER_POC_CONFIG=staging
$ npm run start:dev
```

The `staging` and `prod` profiles require you to modify your system's `hosts` file to add
a mapping from `prod.foo.redhat.com` to your local machine's IP address.  These profiles
will also enable SSL and authentication.

# Build and run (docker)
To run a production build using docker:

```bash
$ npm install
$ npm run prebuild
$ npm run build
$ docker build -t="apicurio/api-designer-poc" --rm .
$ docker run -it -p 8080:80 apicurio/api-designer-poc
```

Then open your browser to:

http://localhost:8080/

## Customizing the container
When running the docker container you can customize it with the following environment
variables:

* `ADS_UI_URL` - The URL to the `ads-ui` federated modules.  Defaults to http://localhost:9009/ads.js
* `AMS_API_URL` - The URL to the Account Management Service API.  Defaults to http://localhost:8001
* `SRS_API_URL` - The URL to the Service Registry Fleet Manager API.  Defaults to http://localhost:8000
* `AUTH_ENABLED` - Whether to enable Keycloak authentication.  Defaults to false
* `KEYCLOAK_REALM` - The Keycloak realm to use for authentication.  Defaults to redhat-external
* `KEYCLOAK_URL` - The Keycloak auth URL to use for authentication.  Defaults to https://sso.redhat.com/auth/
* `KEYCLOAK_SSL_REQUIRED` - The "SSL required" setting for Keycloak authentication.  Defaults to all
* `KEYCLOAK_RESOURCE` - The Keycloak resource to use for authentication.  Defaults to cloud-services
