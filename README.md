# Apache Flink AWS Connectors

This repository contains the official Apache Flink AWS connectors.

## Lyft Fork

This is Lyft's fork of [apache/flink-connector-aws](https://github.com/apache/flink-connector-aws),
based on the upstream `v5.0` branch (Flink 1.19 compatible).

### Branch Strategy

| Branch | Purpose |
|---|---|
| `main` | Tracks upstream `apache/flink-connector-aws` main branch (do not commit Lyft changes here) |
| `lyft-stable-5.0` | Lyft's stable branch with patches applied on top of upstream v5.0 |

### Patches

- **Kinesis GetRecords fetch interval** ([FLINK-36947](https://issues.apache.org/jira/browse/FLINK-36947)):
  Fixes excessive `GetRecords` calls on idle Kinesis sources causing throttling.
  Adds a configurable `READER_NONEMPTY_RECORDS_FETCH_INTERVAL` for non-empty record polling.

### Versioning

Artifact versions follow the pattern `<upstream>-LYFT-<patch>`, e.g. `5.0.0-LYFT-1`.

| Version | When to use |
|---|---|
| `5.0.0-LYFT-1-SNAPSHOT` | Testing -- can be overwritten repeatedly |
| `5.0.0-LYFT-1` | Production release -- immutable once published |

When bumping, increment the patch number: `LYFT-1` → `LYFT-2` → `LYFT-3`.
Keep the upstream prefix (`5.0.0`) matching the upstream tag the branch is based on.

### Publishing

Requires Artifactory publish permissions (see internal documentation).

```bash
# Deploy a SNAPSHOT for testing
mvn versions:set -DnewVersion=5.0.0-LYFT-1-SNAPSHOT -DgenerateBackupPoms=false
rm -f flink-connector-aws/flink-connector-aws-kinesis-streams/dependency-reduced-pom.xml
mvn deploy \
  -pl .,flink-connector-aws-base,flink-connector-aws,flink-connector-aws/flink-connector-aws-kinesis-streams \
  -DskipTests -B -X

# Deploy a release for production
mvn versions:set -DnewVersion=5.0.0-LYFT-1 -DgenerateBackupPoms=false
rm -f flink-connector-aws/flink-connector-aws-kinesis-streams/dependency-reduced-pom.xml
mvn deploy \
  -pl .,flink-connector-aws-base,flink-connector-aws,flink-connector-aws/flink-connector-aws-kinesis-streams \
  -DskipTests -B -X

# Revert version after deploying
mvn versions:set -DnewVersion=5.0.0 -DgenerateBackupPoms=false
```

---

## Apache Flink

Apache Flink is an open source stream processing framework with powerful stream- and batch-processing capabilities.

Learn more about Flink at [https://flink.apache.org/](https://flink.apache.org/)

## Building the Apache Flink AWS Connectors from Source

Prerequisites:

* Unix-like environment (we use Linux, Mac OS X)
* Git
* Maven (we recommend version 3.8.5)
* Java 11

```
git clone https://github.com/apache/flink-connector-aws.git
cd flink-connector-aws
mvn clean package -DskipTests
```

The resulting jars can be found in the `target` directory of the respective module.

## Developing Flink

The Flink committers use IntelliJ IDEA to develop the Flink codebase.
We recommend IntelliJ IDEA for developing projects that involve Scala code.

Minimal requirements for an IDE are:
* Support for Java and Scala (also mixed projects)
* Support for Maven with Java and Scala

### IntelliJ IDEA

The IntelliJ IDE supports Maven out of the box and offers a plugin for Scala development.

* IntelliJ download: [https://www.jetbrains.com/idea/](https://www.jetbrains.com/idea/)
* IntelliJ Scala Plugin: [https://plugins.jetbrains.com/plugin/?id=1347](https://plugins.jetbrains.com/plugin/?id=1347)

Check out our [Setting up IntelliJ](https://nightlies.apache.org/flink/flink-docs-master/flinkDev/ide_setup.html#intellij-idea) guide for details.

## Support

Don’t hesitate to ask!

Contact the developers and community on the [mailing lists](https://flink.apache.org/community.html#mailing-lists) if you need any help.

[Open an issue](https://issues.apache.org/jira/browse/FLINK) if you found a bug in Flink.

## Documentation

The documentation of Apache Flink is located on the website: [https://flink.apache.org](https://flink.apache.org)
or in the `docs/` directory of the source code.

## Fork and Contribute

This is an active open-source project. We are always open to people who want to use the system or contribute to it.
Contact us if you are looking for implementation tasks that fit your skills.
This article describes [how to contribute to Apache Flink](https://flink.apache.org/contributing/how-to-contribute.html).

## About

Apache Flink is an open source project of The Apache Software Foundation (ASF).
The Apache Flink project originated from the [Stratosphere](http://stratosphere.eu) research project.

