---
layout: post
title: "Install Openshift on-premise for development on Mac OS"
category: openshift
tags: 
- Docker
- Kubernetes
- Redhad
- Openshift 
author: Dimas Maryanto
references:
- https://www.okd.io/download.html
- https://www.docker.com
- https://kubernetes.io
comments: true
---

![openshift-logo]({{site.baseurl}}/assets/img/posts/openshift-on-premise-mac/openshift-logo.png)

Untuk menginstall openshift on-premise, Oh ia berikut adalah spesifikasi laptop saya yaitu `Macbook Pro 13" Dual Core Intel Core i5, 8Gb of Ram, and 256 SSD Storage` yang saya gunakan untuk Install Openshift.

Platform yang kita gunakan untuk mencoba Openshift ini adalah `OKD is a distribution of Kubernetes optimized for continuous application development and multi-tenant deployment` atau silahkan kunjungi [website](https://www.okd.io/)

<!--more-->

Karena kita di PT. Tabeldata Informatika partner dari PT. Multipolar Technology yang merupakan Brand Ambasador dari IBM dan RedHat, so jadi application yang kita dukung / develop harus support dengan system PaaS (Platform as a service) seperti Openshift. 

Ada 2 cara untuk menginstall Openshift client tools yaitu dengan `Run OKD in a Container` dan `Run the All-In-One VM with Minishift` tapi disini kita installnya dengan method yang paling simple aja ya...

Nah sekarang kita install Openshift di Mac OS, ada berapa yang kita butuhkan untuk menginstall yaitu:
- Install docker
    - Config update VM docker settings
    - Setup `network-insecure-registry`
- Install VirtualBox
- Install kubernetes CLI
- Install Openshift cluster
- Install Minishift

## Instal Docker

Untuk Mac OS, install docker cukup mudah kita hanya download Docker Desktop dan install, it's done!, silahkan [download disini](https://docs.docker.com/docker-for-mac/install/)

hasinya seperti berikut: 

```bash
Client: Docker Engine - Community
 Version:           19.03.5
 API version:       1.40
 Go version:        go1.12.12
 Git commit:        633a0ea
 Built:             Wed Nov 13 07:22:34 2019
 OS/Arch:           darwin/amd64
 Experimental:      false

Server: Docker Engine - Community
 Engine:
  Version:          19.03.5
  API version:      1.40 (minimum version 1.12)
  Go version:       go1.12.12
  Git commit:       633a0ea
  Built:            Wed Nov 13 07:29:19 2019
  OS/Arch:          linux/amd64
  Experimental:     false
 containerd:
  Version:          v1.2.10
  GitCommit:        b34a5c8af56e510852c35414db4c1f4fa6172339
 runc:
  Version:          1.0.0-rc8+dev
  GitCommit:        3e425f80a8c931f88e6d94a8c831b9d5aa481657
 docker-init:
  Version:          0.18.0
  GitCommit:        fec3683
```

## Docker Config settings

Untuk menjalankan openshift dengan keterbatasan resource di laptop saya ini, configurasi yang saya gunakan adalah seperti berikut:

- `Docker menu -> Preferences -> Resources -> Advanced`
![docker-preferences-resource]({{site.baseurl}}/assets/img/posts/openshift-on-premise-mac/docker-pref-resources.png)

- `Docker menu -> Preferences -> Docker Engine`
```json
{
    "experimental": false,
    "insecure-registries": [
        "172.30.0.0/16" // add this ip-address network to your insecure-registry
    ],
    "debug": true
}
```

Setelah semua selesai kemudian `Apply & Restart`

## Install Kubernetes CLI

Untuk install kubernetes cli, kita bisa install menggunakan homebrew berikut perintahnya:

```bash
brew install kubernetes-cli
```

berikut hasilnya:

```bash
➜ kubectl version
Client Version: version.Info 
{   Major:"1", 
    Minor:"15", 
    GitVersion:"v1.15.5", 
    GitCommit:"20c265fef0741dd71a66480e35bd69f18351daea", 
    GitTreeState:"clean", 
    BuildDate:"2019-10-15T19:16:51Z", 
    GoVersion:"go1.12.10", 
    Compiler:"gc", 
    Platform:"darwin/amd64"
}
```

## Install Openshift

Untuk install openshift client tools, kita bisa menggunakan homebrew berikut perintahnya:

```bash
brew install openshift-cli
```

Setelah itu berikut hasilnya:

```bash
➜  oc version
Client Version: version.Info{
  Major:"4", 
  Minor:"1+", 
  GitVersion:"v4.1.0+b4261e0", 
  GitCommit:"b4261e07ed", 
  GitTreeState:"clean", 
  BuildDate:"2019-10-06T23:21:44Z", 
  GoVersion:"go1.13.1", 
  Compiler:"gc", 
  Platform:"darwin/amd64"
}
The connection to the server 127.0.0.1:8443 was refused - did you specify the right host or port?
```

## Install minishift

Tetapi dilognya openshift client tools masih ada yang missing yaitu openshift clusternya blum running, nah sekarang kita install dengan menggunakan `minishift`. Untuk menginstall minishift kita juga bisa menggunakan homebrew berikut perintahnya:

```bash
brew cask install minishift
```

Berikut hasilnya:

```bash
➜  minishift version
minishift v1.34.2+83ebaab
```

## Create Cluster Openshift

Untuk menjalankan platform Openshift berikut perintahnya:

```bash
minishift start --vm-driver=virtualbox
```

berikut hasilnya:

```bash
-- Starting profile 'minishift'
-- Check if deprecated options are used ... OK
-- Checking if https://github.com is reachable ... OK
-- Checking if requested OpenShift version 'v3.11.0' is valid ... OK
-- Checking if requested OpenShift version 'v3.11.0' is supported ... OK
-- Checking if requested hypervisor 'virtualbox' is supported on this platform ... OK
-- Checking if VirtualBox is installed ... OK
-- Checking the ISO URL ... OK
-- Checking if provided oc flags are supported ... OK
-- Starting the OpenShift cluster using 'virtualbox' hypervisor ...
-- Starting Minishift VM ........................... OK
-- Checking for IP address ... OK
-- Checking for nameservers ... OK
-- Checking if external host is reachable from the Minishift VM ... 
   Pinging 8.8.8.8 ... OK
-- Checking HTTP connectivity from the VM ... 
   Retrieving http://minishift.io/index.html ... OK
-- Checking if persistent storage volume is mounted ... OK
-- Checking available disk space ... 20% used OK
-- Writing current configuration for static assignment of IP address ... OK
-- OpenShift cluster will be configured with ...
   Version: v3.11.0
-- Copying oc binary from the OpenShift container image to VM ... OK
-- Starting OpenShift cluster .........................................
Getting a Docker client ...
Checking if image openshift/origin-control-plane:v3.11.0 is available ...
Checking type of volume mount ...
Determining server IP ...
Using public hostname IP 192.168.99.101 as the host IP
Checking if OpenShift is already running ...
Checking for supported Docker version (=>1.22) ...
Checking if insecured registry is configured properly in Docker ...
Checking if required ports are available ...
Checking if OpenShift client is configured properly ...
Checking if image openshift/origin-control-plane:v3.11.0 is available ...
Starting OpenShift using openshift/origin-control-plane:v3.11.0 ...
I0203 10:25:41.154186    2554 flags.go:30] Running "create-kubelet-flags"
I0203 10:25:41.576938    2554 run_kubelet.go:49] Running "start-kubelet"
I0203 10:25:41.797440    2554 run_self_hosted.go:181] Waiting for the kube-apiserver to be ready ...
I0203 10:26:34.864832    2554 interface.go:26] Installing "kube-proxy" ...
I0203 10:26:34.865570    2554 interface.go:26] Installing "kube-dns" ...
I0203 10:26:34.865579    2554 interface.go:26] Installing "openshift-service-cert-signer-operator" ...
I0203 10:26:34.865583    2554 interface.go:26] Installing "openshift-apiserver" ...
I0203 10:26:34.865618    2554 apply_template.go:81] Installing "openshift-apiserver"
I0203 10:26:34.866478    2554 apply_template.go:81] Installing "kube-proxy"
I0203 10:26:34.867838    2554 apply_template.go:81] Installing "kube-dns"
I0203 10:26:34.868211    2554 apply_template.go:81] Installing "openshift-service-cert-signer-operator"
I0203 10:27:10.721206    2554 interface.go:41] Finished installing "kube-proxy" "kube-dns" "openshift-service-cert-signer-operator" "openshift-apiserver"
I0203 10:27:18.757285    2554 run_self_hosted.go:242] openshift-apiserver available
I0203 10:27:18.757354    2554 interface.go:26] Installing "openshift-controller-manager" ...
I0203 10:27:18.757378    2554 apply_template.go:81] Installing "openshift-controller-manager"
I0203 10:27:21.611056    2554 interface.go:41] Finished installing "openshift-controller-manager"
Adding default OAuthClient redirect URIs ...
Adding sample-templates ...
Adding persistent-volumes ...
Adding web-console ...
Adding registry ...
Adding router ...
Adding centos-imagestreams ...
I0203 10:27:21.639431    2554 interface.go:26] Installing "sample-templates" ...
I0203 10:27:21.639439    2554 interface.go:26] Installing "persistent-volumes" ...
I0203 10:27:21.639448    2554 interface.go:26] Installing "openshift-web-console-operator" ...
I0203 10:27:21.639454    2554 interface.go:26] Installing "openshift-image-registry" ...
I0203 10:27:21.639459    2554 interface.go:26] Installing "openshift-router" ...
I0203 10:27:21.639464    2554 interface.go:26] Installing "centos-imagestreams" ...
I0203 10:27:21.639512    2554 apply_list.go:67] Installing "centos-imagestreams"
I0203 10:27:21.639657    2554 interface.go:26] Installing "sample-templates/mysql" ...
I0203 10:27:21.639663    2554 interface.go:26] Installing "sample-templates/postgresql" ...
I0203 10:27:21.639668    2554 interface.go:26] Installing "sample-templates/dancer quickstart" ...
I0203 10:27:21.639672    2554 interface.go:26] Installing "sample-templates/django quickstart" ...
I0203 10:27:21.639676    2554 interface.go:26] Installing "sample-templates/rails quickstart" ...
I0203 10:27:21.639681    2554 interface.go:26] Installing "sample-templates/mongodb" ...
I0203 10:27:21.639686    2554 interface.go:26] Installing "sample-templates/mariadb" ...
I0203 10:27:21.639690    2554 interface.go:26] Installing "sample-templates/cakephp quickstart" ...
I0203 10:27:21.639694    2554 interface.go:26] Installing "sample-templates/nodejs quickstart" ...
I0203 10:27:21.639699    2554 interface.go:26] Installing "sample-templates/jenkins pipeline ephemeral" ...
I0203 10:27:21.639703    2554 interface.go:26] Installing "sample-templates/sample pipeline" ...
I0203 10:27:21.639740    2554 apply_list.go:67] Installing "sample-templates/sample pipeline"
I0203 10:27:21.640423    2554 apply_template.go:81] Installing "openshift-web-console-operator"
I0203 10:27:21.641009    2554 apply_list.go:67] Installing "sample-templates/mysql"
I0203 10:27:21.641116    2554 apply_list.go:67] Installing "sample-templates/postgresql"
I0203 10:27:21.641200    2554 apply_list.go:67] Installing "sample-templates/dancer quickstart"
I0203 10:27:21.641279    2554 apply_list.go:67] Installing "sample-templates/django quickstart"
I0203 10:27:21.641364    2554 apply_list.go:67] Installing "sample-templates/rails quickstart"
I0203 10:27:21.641509    2554 apply_list.go:67] Installing "sample-templates/mongodb"
I0203 10:27:21.641643    2554 apply_list.go:67] Installing "sample-templates/mariadb"
I0203 10:27:21.641720    2554 apply_list.go:67] Installing "sample-templates/cakephp quickstart"
I0203 10:27:21.641799    2554 apply_list.go:67] Installing "sample-templates/nodejs quickstart"
I0203 10:27:21.641877    2554 apply_list.go:67] Installing "sample-templates/jenkins pipeline ephemeral"
I0203 10:27:31.275676    2554 interface.go:41] Finished installing "sample-templates/mysql" "sample-templates/postgresql" "sample-templates/dancer quickstart" "sample-templates/django quickstart" "sample-templates/rails quickstart" "sample-templates/mongodb" "sample-templates/mariadb" "sample-templates/cakephp quickstart" "sample-templates/nodejs quickstart" "sample-templates/jenkins pipeline ephemeral" "sample-templates/sample pipeline"
I0203 10:28:00.442845    2554 interface.go:41] Finished installing "sample-templates" "persistent-volumes" "openshift-web-console-operator" "openshift-image-registry" "openshift-router" "centos-imagestreams"
Server Information ...
OpenShift server started.

The server is accessible via web console at:
    https://192.168.99.101:8443/console


Could not set oc CLI context for 'minishift' profile: Error during setting 'minishift' as active profile: The specified path to the kube config '/Users/dimasm93/.minishift/machines/minishift_kubeconfig' does not exist
```

Sekarang coba buka `https://minishift-ip-cluster:8443/console` dan trush certificate maka hasilnya seperti berikut:

![okd-login]({{site.baseurl}}/assets/img/posts/openshift-on-premise-mac/openshift-login.png)

Untuk user kita bisa menggunakan `developer` dan passwordnya `developer` kemudian login, setelah login maka akan muncul halaman seperti berikut:

![openshift-catalog]({{site.baseurl}}/assets/img/posts/openshift-on-premise-mac/openshift-catalog.png)

## Management cluster openshift

Untuk management cluster kita bisa menggunakan command seperti berikut:

```bash
minishift --help
Minishift is a command-line tool that provisions and manages single-node OpenShift clusters optimized for development workflows.

Usage:
  minishift [command]

Available Commands:
  addons      Manages Minishift add-ons.
  completion  Outputs minishift shell completion for the given shell (bash or zsh)
  config      Modifies Minishift configuration properties.
  console     Opens or displays the OpenShift Web Console URL.
  delete      Deletes the Minishift VM.
  docker-env  Sets Docker environment variables.
  help        Help about any command
  hostfolder  Manages host folders for the Minishift VM.
  image       Exports and imports container images.
  ip          Gets the IP address of the running cluster.
  logs        Gets the logs of the running OpenShift cluster.
  oc-env      Sets the path of the 'oc' binary.
  openshift   Interacts with your local OpenShift cluster.
  profile     Manages Minishift profiles.
  services    Manage Minishift services
  setup       Configures pre-requisites for Minishift on the host machine
  ssh         Log in to or run a command on a Minishift VM with SSH.
  start       Starts a local OpenShift cluster.
  status      Gets the status of the local OpenShift cluster.
  stop        Stops the running local OpenShift cluster.
  update      Updates Minishift to the latest version.
  version     Gets the version of Minishift.

Flags:
      --alsologtostderr                  log to standard error as well as files
  -h, --help                             help for minishift
      --log_backtrace_at traceLocation   when logging hits line file:N, emit a stack trace (default :0)
      --log_dir string                   If non-empty, write log files in this directory (default "")
      --logtostderr                      log to standard error instead of files
      --profile string                   Profile name (default "minishift")
      --show-libmachine-logs             Show logs from libmachine.
      --stderrthreshold severity         logs at or above this threshold go to stderr (default 2)
  -v, --v Level                          log level for V logs. Level varies from 1 to 5 (default 1).
      --vmodule moduleSpec               comma-separated list of pattern=N settings for file-filtered logging

Use "minishift [command] --help" for more information about a command.
```