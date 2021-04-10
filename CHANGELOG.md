# Changelog

All notable changes to Docker resources for WSO2 API Microgateway will be documented in this file.

The format is based on [Keep a Changelog](https://keepachangelog.com/en/1.0.0/)

## [v3.2.1.2] - 2021-04-10

### Changed

- Upgrade base image packages during the build (refer to [issue](https://github.com/wso2/docker-mg/issues/9))

## [v3.2.1.1] - 2020-10-27

### Changed

- Upgrade to jdk8u265-b01 (refer to [issue](https://github.com/wso2/docker-mg/issues/4))
- Upgrade ubuntu base image to 20.04 (refer to [issue](https://github.com/wso2/docker-mg/issues/4))
- Upgrade to API Microgateway 3.2.1 (refer to [issue](https://github.com/wso2/docker-mg/issues/5))
- Remove `--no-check-certificate` from DockerFiles (refer to [issue](https://github.com/wso2/docker-mg/issues/6))

For detailed information on the tasks carried out during this release, please see the GitHub milestone
[v3.2.1.1](https://github.com/wso2/docker-mg/milestone/2?closed=1).

## [v3.2.0] - 2020-10-12

### Environments

- Successful evaluation using Docker Engine Community version `19.03.12` (both client and server)

### Added

- Alpine and Ubuntu based Docker resources for WSO2 API Microgateway (refer to [issue](https://github.com/wso2/docker-mg/issues/2))

For detailed information on the tasks carried out during this release, please see the GitHub milestone
[v3.2.0](https://github.com/wso2/docker-mg/milestone/1?closed=1).

[v3.2.0]: https://github.com/wso2/docker-mg/compare/9405bc5...v3.2.0
[v3.2.1.1]: https://github.com/wso2/docker-mg/compare/v3.2.0...v3.2.1.1
[v3.2.1.2]: https://github.com/wso2/docker-mg/compare/v3.2.1.1...v3.2.1.2
