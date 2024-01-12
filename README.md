# console-disable-m3
Dynamic plugin for OpenShift Console to disable metal3 static plugin

## Local development

In one terminal window, run:

1. `yarn install`
2. `yarn run start`

In another terminal window, run:

1. `oc login`
2. `yarn run start-console` (requires [Docker](https://www.docker.com) or [podman](https://podman.io))

This will run the OpenShift console in a container connected to the cluster
you've logged into. The plugin HTTP server runs on port 9001 with CORS enabled.
Navigate to <http://localhost:9000/customization> to see the running plugin.

## Deployment on cluster

You can deploy the plugin to a cluster by applying `manifest.yaml`.

```sh
oc apply -f manifest.yaml
```

Once deployed, patch the
[Console operator](https://github.com/openshift/console-operator)
config to enable the plugin.

```sh
oc patch consoles.operator.openshift.io cluster --patch '{ "spec": { "plugins": ["console-disable-m3"] } }' --type=merge
```

## Docker image

1. Build the image:
   ```sh
   docker build -t quay.io/$USER/console-disable-m3:latest .
   ```
2. Run the image:
   ```sh
   docker run -it --rm -d -p 9001:80 quay.io/$USER/console-disable-m3:latest
   ```
3. Push the image to image registry:
   ```sh
   docker push quay.io/$USER/console-disable-m3:latest
   ```

Update and apply `manifest.yaml` to use a custom plugin image.
