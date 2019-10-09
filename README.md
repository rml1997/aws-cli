# OCI CLI in Docker

Containerized OCI CLI on alpine to avoid requiring the oci cli to be installed on CI machines.

## Build

```
docker build -t rml1997/oci-cli .
```

Automated build on Docker Hub

[![DockerHub Badge](http://dockeri.co/image/rml1997/oci-cli)](https://hub.docker.com/r/rml1997/oci-cli/)

## Usage

Configure:

```
export OCI_ACCESS_KEY_ID="<id>"
export OCI_SECRET_ACCESS_KEY="<key>"
export OCI_DEFAULT_REGION="<region>"
```

Upload file to S3:

```
./oci.sh s3 cp ../dcos-centos-virtualbox-0.2.1.box s3://downloads.dcos.io/dcos-vagrant/
```

Caveat: Because `oci.sh` mounts the current directory as `/project`, paths to local files must be relative to the current directory.

## Install

To use `oci.sh` as a drop-in replacement for calls to the aws-cli, use one of the following methods:

Add an alias to your shell:

```
alias oci='docker run --rm -t $(tty &>/dev/null && echo "-i") -e "AWS_ACCESS_KEY_ID=${AWS_ACCESS_KEY_ID}" -e "AWS_SECRET_ACCESS_KEY=${AWS_SECRET_ACCESS_KEY}" -e "AWS_DEFAULT_REGION=${AWS_DEFAULT_REGION}" -v "$(pwd):/project" rml1997/oci-cli'
```

Or drop it into your path named `oci`:

```
curl -o /usr/local/bin/oci https://raw.githubusercontent.com/rml1997/oci-cli/master/oci.sh && chmod a+x /usr/local/bin/oci
```

## Maintenance 

- The Docker image build & publish is automated by DockerHub for master commits and tags.
- The ocicli and s3cmd packages have handcoded versions in the Dockerfile that need to be bumped manually.

## References

OCI CLI Docs: https://docs.cloud.oracle.com/iaas/Content/API/Concepts/cliconcepts.htm


# License
Copyright 2019-2020 Rob Levy
Copyright 2016-2017 Mesosphere, Inc.

Licensed under the Apache License, Version 2.0 (the "License");
you may not use this repository except in compliance with the License.

The contents of this repository are solely licensed under the terms described in the [LICENSE file](./LICENSE) included in this repository.

Unless required by applicable law or agreed to in writing, software
distributed under the License is distributed on an "AS IS" BASIS,
WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND, either express or implied.
See the License for the specific language governing permissions and
limitations under the License.
