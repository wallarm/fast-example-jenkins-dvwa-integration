# fast-examples-dvwa

Example of Wallarm FAST integration with the DVWA using selenium tests in a Jenkins environment. This example builds upon the local example (https://github.com/wallarm/fast-example-dvwa)

Read more about Wallarm FAST: https://wallarm.com/products/fast
For full FAST documentation visit https://docs.fast.wallarm.com/

## Jenkins integration example:

Jenkins must have access to `sudo` and be able to run `docker-compose`

Use this repository as the git source (https://github.com/wallarm/fast-example-jenkins-dvwa-integration)

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

The changes ensure the build step does cleanup in case of failure. You may set the Wallarm TOKEN in the script or use the Jenkins build params

### Jenkins preconfigured local example:

The preconfigured Jenkins example requires Jenkins to have access to `sudo` and `docker-compose`, which are lacking in the official Jenkins images. To ease the running of the provided example, you may use a pre-built image with the required components already present. One such image with startup instructions can be found here: https://github.com/fabianenardon/jenkins-docker-demo (requires installation of github plugin, uses the docker found on the host machine)

To run the jenkins example locally copy the contents of `jenkins_home` into your own Jenkins folder. Then check the configuration of the pipeline: both git and buildsteps should already be set as described above.

### Jenkins public demo:

A pre-configured and running Jenkins example is available to view and run builds with at https://jenkinsfast.demo.wallarm.com/ (user:demo, pass:demo). You may launch builds with your Wallarm TOKEN (and monitor the progress at https://my.wallarm.com/testing).
