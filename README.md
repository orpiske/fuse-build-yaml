# Fuse Build Yaml

This project is used to generate the YAML file that is consumed by [PiG](https://gitlab.cee.redhat.com/tcunning/piglet)
to generate the configuration and drive the PNC builds.

## Usage

This project supports the modularized format of the Fuse project that started with version
7.5. As such, it is capable of generating different yaml files required by each module or
product. The generated files can be distinguished by their classifiers, which match the
Maven profile used to generate the files.

The following Maven profiles are currently implemented:

* core-sb1: for Fuse Core modules with Spring Boot v1
* dv: for the Data Virtualization project
* fusefis: for the legacy Fuse FIS project format (likely to be removed)
* ipaas: for iPaaS
* springboot2: for Fuse components built with Spring Boot v2 support.

A sample generation of the files from this project, for the `core-sb1` profile looks like this:

```mvn -Pcore-sb1 -Dbuild.stage=DR1 -Dbuild.milestone=DR1 -DaltDeploymentRepository=local-snapshots::default::http://nexus:8081/artifactory/snaphots clean deploy```

This would generate the following set of files:

```fuse-build-yaml-7.5.0.redhat-SNAPSHOT-core-sb1-deptree.html
fuse-build-yaml-7.5.0.redhat-SNAPSHOT-core-sb1-modified-yaml.yaml
fuse-build-yaml-7.5.0.redhat-SNAPSHOT-core-sb1-staging-deptree.html
fuse-build-yaml-7.5.0.redhat-SNAPSHOT-core-sb1-staging-modified-yaml.yaml
fuse-build-yaml-7.5.0.redhat-SNAPSHOT-core-sb1-staging-yaml.yaml
fuse-build-yaml-7.5.0.redhat-SNAPSHOT-core-sb1-yaml.yaml
```

Similarly, using a different profile, such as `ipaas`, would generate the files specifically for that profile:

```mvn -Pipaas -Dbuild.stage=DR1 -Dbuild.milestone=DR1 -DaltDeploymentRepository=local-snapshots::default::http://nexus:8081/artifactory/snaphots clean deploy```

```fuse-build-yaml-7.5.0.redhat-SNAPSHOT-ipaas-deptree.html
fuse-build-yaml-7.5.0.redhat-SNAPSHOT-ipaas-modified-yaml.yaml
fuse-build-yaml-7.5.0.redhat-SNAPSHOT-ipaas-staging-deptree.html
fuse-build-yaml-7.5.0.redhat-SNAPSHOT-ipaas-staging-modified-yaml.yaml
fuse-build-yaml-7.5.0.redhat-SNAPSHOT-ipaas-staging-yaml.yaml
fuse-build-yaml-7.5.0.redhat-SNAPSHOT-ipaas-yaml.yaml
```

There is no default profile and not providing one will result in no file to be generated.

Please also make sure to inform the appropriate value (ie.: DR1, DR2, CR1, CR2, etc) for
build.milestone and build.stage properties. These are used to fill certain parameters in
the YAML files. Among other things, they define the current milestone on PNC and are used
in the title of the build groups.
