# fig-ci-test-app

This example app shows you how to add a `fig.yml` file to your project so you can test it in your build/CI pipeline using [fig-buildbox-agent](https://github.com/toolmantim/fig-buildbox-agent).

The [fig.yml](fig.yml) defines the entire environment for your app:

```yml
app:
  build: .
  links:
    - db:db
  environment:
    PGHOST: db
    PGUSER: postgres
db:
  image: postgres
```

* `app` is built from the [Dockerfile](Dockerfile) and is just an Ubuntu image with the postgresql client installed.
* `db` is just the [standard Postgresql server image](https://registry.hub.docker.com/_/postgres/), and is linked to `app` using the alias `db`.
* The environment variables tell the tests where it can find the linked Postgresql server.

Your fig.yml for buildbox is exactly the same one you'd use with Fig for local development, but **the container containing your test scripts must be called app**. `app` is where fig-buildbox-agent will try to run your tests from inside of (using `fig run app <build-script>`).

See the [Fig documentation](http://www.fig.sh/) for example configurations for various langauges and frameworks, as well [the fig.yml options](http://www.fig.sh/yml.html).

See [fig-buildbox-agent](https://github.com/toolmantim/fig-buildbox-agent) for how to run your Docker-enabled application in your Buildbox build pipeline.

## Using

* Create a new project in your Buildbox account:

![image](https://cloud.githubusercontent.com/assets/153/4101095/b15fee78-30d0-11e4-8a1a-254b086664c5.png)

* Configure the build pipeline to have two parallel steps for `scripts/tests_1` and `scripts/tests_2`:

![image](https://cloud.githubusercontent.com/assets/153/4101094/a0b6faee-30d0-11e4-922d-0eca094b9488.png)

* Set your project's build agent query rules to only use `fig=true` build agents.
* Create the project
* Start two or more fig-buildbox-agent instances
* Trigger a build

The agents will detect the build jobs and execute their respective test script in parallel using completely isolated Docker environments.

![image](https://cloud.githubusercontent.com/assets/153/4101107/b8f9bee2-30d1-11e4-97f6-4468622c080d.png)
