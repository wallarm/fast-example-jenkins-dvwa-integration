# fast-examples-dvwa

Example of Wallarm FAST integration with the DVWA using Selenium tests in the Jenkins environment. This example builds upon the local example (https://github.com/wallarm/fast-example-dvwa).

Learn more about Wallarm FAST: https://wallarm.com/products/fast.
For full FAST documentation, visit https://docs.fast.wallarm.com.

## Jenkins integration example:

Jenkins must have access to `sudo` and be able to run `docker-compose`.

Use this repository as the git source: https://github.com/wallarm/fast-example-jenkins-dvwa-integration.

Repeat the steps in local run example, but instead of launching the script, copy this modified version into your build step:

```
{
  sudo -E docker-compose build && \
  sudo -E docker-compose up -d dvwa fast selenium && \
  sudo -E docker-compose run test && \
  sudo -E docker-compose run --rm -e CI_MODE=testing -e TEST_RUN_URI=http://dvwa:80 fast && \

  sudo -E docker-compose down --remove-orphans
} || {
  sudo -E docker-compose down --remove-orphans
  exit 1
}
```

The changes ensure the build step does cleanup in case of failure. You may set the Wallarm TOKEN in the script or use the Jenkins build parameters.

### Jenkins preconfigured local example

The preconfigured Jenkins example requires that Jenkins has access to `sudo` and `docker-compose`, which are lacking in the official Jenkins images. To ease of trying out this example, you can use a pre-built image with all the required configuration in place. For examples, [this one](https://github.com/fabianenardon/jenkins-docker-demo). It uses Docker on the host machine and requires installation of the GitHub plugin.

To run the Jenkins example locally, copy the contents of `jenkins_home` into your own Jenkins folder. Then check the configuration of the pipeline: both git and buildsteps should already be set as described above.

### Jenkins live demo

A pre-configured and running Jenkins example is available at https://jenkinsfast.demo.wallarm.com/ (user:demo, pass:demo). 

You can launch builds with your Wallarm TOKEN (and monitor the progress at https://us1.my.wallarm.com/testing).
