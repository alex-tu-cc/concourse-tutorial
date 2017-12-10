# Concourse Tutorial

Learn to use https://concourse.ci with this linear sequence of tutorials. Learn each concept that builds on the previous concept.

Read the tutorial at https://concoursetutorial.com

## Thanks

Thanks to Alex Suraci for inventing Concourse CI, and to Pivotal for sponsoring him and a team of developers to work since 2014.

At Stark & Wayne we started this tutorial as we were learning Concourse in early 2015, and we've been using Concourse in production since mid-2015 internally and at nearly all client projects.

Thanks to everyone who has worked through this tutorial and found it useful. I love learning that you're enjoying the tutorial and enjoying Concourse.

Thanks for all the pull requests to help fix regressions with some Concourse versions that came out with "backwards incompatible change".

## Getting Started

### Mac & Linux

1. Install [Virtualbox](https://www.virtualbox.org/wiki/Downloads).
2. Install [BOSH CLI](https://bosh.io/docs/cli-v2.html#install)

    For Mac:

    ```
    brew install cloudfoundry/tap/bosh-cli
    ```

    For Linux:

    ```
    wget -q -O - https://raw.githubusercontent.com/starkandwayne/homebrew-cf/master/public.key | apt-key add -
    echo "deb http://apt.starkandwayne.com stable main" | tee /etc/apt/sources.list.d/starkandwayne.list
    apt-get update
    apt-get install bosh-cli
    ```

3. Setup a Single VM concourse using Virtualbox and BOSH.

    Download the `concourse-lite` deployment manifest and then have `bosh` create a
    Single VM server running concourse on Virtualbox.

    ```
    wget https://github.com/starkandwayne/concourse-tutorial/tree/master/manifests/concourse-lite.yml
    bosh create-env concourse-lite.yml
    ```

### Windows

1. Use the Vagrant box as a pre-compiled build of a Single VM instance of Concourse.

```
git clone https://github.com/starkandwayne/concourse-tutorial.git
cd concourse-tutorial
vagrant box add concourse/lite --box-version $(cat VERSION)
vagrant up
```

### Test Setup

Open http://192.168.100.4:8080/ in your browser:

[![initial](no_pipelines.png)](http://192.168.100.4:8080/)

Once the page loads in your browser, click to download the `fly` CLI appropriate for your operating system:

![cli](fly_cli.png)

Once downloaded, copy the `fly` binary into your path (`$PATH`), such as `/usr/local/bin` or `~/bin`. Don't forget to also make it executable. For example,

```
sudo mkdir -p /usr/local/bin
sudo mv ~/Downloads/fly /usr/local/bin
sudo chmod 0755 /usr/local/bin/fly
```

For Windows users, use [this article](https://stackoverflow.com/questions/23400030/windows-7-add-path)
to see where to add `fly` in to the `PATH`.

## Target Concourse

In the spirit of declaring absolutely everything you do to get absolutely the same result every time, the `fly` CLI requires that you specify the target API for every `fly` request.

First, alias it with a name `tutorial` (this name is used by all the tutorial task scripts):

```
fly --target tutorial login --concourse-url http://192.168.100.4:8080
fly --target tutorial sync
```

You can now see this saved target Concourse API in a local file:

```
cat ~/.flyrc
```

Shows a simple YAML file with the API, credentials etc:

```yaml
targets:
  tutorial:
    api: http://192.168.100.4:8080
    token:
      type: ""
      value: ""
```

When we use the `fly` command we will target this Concourse API using `fly --target tutorial`.

> @alexsuraci: I promise you'll end up liking it more than having an implicit target state :) Makes reusing commands from shell history much less dangerous (rogue fly configure can be bad)

