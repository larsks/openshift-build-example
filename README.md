# OpenShift Build Example

## Demo

Create the resources:

```
oc apply -k .
```

This triggers the following sequence of actions:

1. The parent and child `BuildConfigs` will trigger because they both have a `ConfigChange` trigger defined.
2. The parent `Build` starts immediately, but the child `Build` blocks until the parent `Build` is complete.
3. The parent `Build` completes, which has two outcomes:
  1. The child `Build` starts
  2. The parent `Deployment` is triggered due to the `image.openshift.io/triggers` annotation
4. The child `Build` completes, triggering the child `Deployment`

Once everything has settled, there will be two pods running, exposing a web server via the associated `Route` resources.

## Building from an ImageStream

The child `BuildConfig` instructs OpenShift to build from the parent image using an `ImageStreamTag` reference:

```
strategy:
  type: Docker
  dockerStrategy:
    dockerfilePath: Dockerfile
    from:
      kind: ImageStreamTag
      name: openshift-build-example:parent
```

This causes OpenShift to **replace** the `FROM` line in the `Dockerfile`, so the actual value of `FROM` in the `Dockerfile` is irrelevant. In this example I've set it like this:

```
FROM will-be-replaced
```
