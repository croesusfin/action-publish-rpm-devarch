# action-publish-rpm-devarch

Composite action to transfer a `RPM` file to devarch (Croesus) using `SFTP`.

> Note: This action can only be run on the `builddocker` runner.

Documentation:

- [Creating a composite action](https://docs.github.com/en/actions/creating-actions/creating-a-composite-action)
- [sftp(1) - Linux man page](https://linux.die.net/man/1/sftp)

## Example

```yaml
name: Example workflow yml

on: [push]

jobs:
  build:
    runs-on: [self-hosted, builddocker] 
    steps:
    - name: Publish Portfolio RPM
      uses: croesusfin/action-publish-rpm-devarch@v1
      with:
        project-name: Portfolio
        version: 8.8.9-88245_pf1234
        rpm-artifact-name: portfolio-rpm
        devarch-user: superuser1
```

## Description

Here is what this action do:

1. Download the `RPM` file from the artifact

   Using the [actions/download-artifact](https://github.com/actions/download-artifact), the action will download the `RPM` located in the artifact specified.

2. Transfer the RPM file using `SFTP`

   Using `SFTP`, transfer the `RPM` downloaded previously.

   If the folder `/data/archives/{project-name}` does not exist, the action will fail. This folder MUST be created prior the execution of this test.

   If the folder `/data/archives/{project-name}/{version}` or `/data/archives/{project-name}/{version}/rpm` already exist, the action will fail. This is to prevent overwriting existing `RPM` package.

## Inputs and Outputs

See [action.yml](./action.yaml).

## Contributing

Feel free to open a pull request!

- [CONTRIBUTING.md](./CONTRIBUTING.md)
- [PULL_REQUESTS.md](./PULL_REQUESTS.md)

## How to release

See [Using tags for release management](https://docs.github.com/en/actions/creating-actions/about-custom-actions#using-tags-for-release-management)

Tag a commit with an appropriate version following [semver](https://semver.org/).
