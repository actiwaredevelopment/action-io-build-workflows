# ACTION: IO Build Workflows

In this repository workflows are offered, which can be used to build the projects or services of ACTIWARE.IO.

## Build workflows for .net IO Modules

### Build .NET 4.x Application (e.g. an desktop extension)

```yml
jobs:
  build-desktop-ext:
    uses: actiwaredevelopment/action-io-build-workflows/.github/workflows/build-dotnet-desktop-app.yml@main
    with:
      sln-file: src\desktop-extension\Module.Barcode.DesktopExtension.sln
      artifact-name: DesktopExtensionFiles
      artifact-files: |
        module-definition/functions/*
        module-definition/install/*
        module-definition/references/*
```

### Build windows

```yml
jobs:
  build:
    uses: actiwaredevelopment/action-io-build-workflows/.github/workflows/build-dotnet-io-module.yml@main
    with:
      repository: ${{ github.repository }}
      branch: ${{ github.ref_name }}
      dotnet-version: 6.0.x
      project: src/service-v2-dotnet/service/io2-module-template-service.csproj
      modulefile: "module.zip"
      dockerfile: "./.github/docker/dockerfile"
      ui-build-path: "./src/configuration/build"
      rid: "win-x64"
      create-single-file: true
      module-artifact: "DesktopExtension" # Optional: downloads the given artifact, before creating module zip
      module-artifact-path: "./" # Optional: where to download the module-artifact
    secrets:
      action-user: ${{ secrets.GH_ACTION_USER }}
      action-token: ${{ secrets.GH_ACTION_TOKEN }}
      awdev-nuget-source: ${{ secrets.AWDEV_NUGET_URL }}
      npm-package-token: ${{ secrets.AWDEV_NPM_PACKAGE_TOKEN }}
      npmrc-config: ${{ secrets.NPMRC_CONFIG }}

  use-download:
    name: Use download
    needs: build
    runs-on: ubuntu-latest

    steps:
      - name: "Download Artifact"
        uses: actions/download-artifact@v2.0.8
        with:
          name: ${{ needs.build.outputs.downloads }}
```

### Build linux

```yml
jobs:
  build:
    uses: actiwaredevelopment/action-io-build-workflows/.github/workflows/build-dotnet-io-module.yml@main
    with:
      repository: ${{ github.repository }}
      branch: ${{ github.ref_name }}
      dotnet-version: 6.0.x
      project: src/service-v2-dotnet/service/io2-module-template-service.csproj
      modulefile: "module.zip"
      dockerfile: "./.github/docker/dockerfile"
      ui-build-path: "./src/configuration/build"
      rid: "linux-x64"
      create-single-file: false
      module-artifact: "DesktopExtension" # Optional: downloads the given artifact, before creating module zip
      module-artifact-path: "./" # Optional: where to download the module-artifact
    secrets:
      action-user: ${{ secrets.GH_ACTION_USER }}
      action-token: ${{ secrets.GH_ACTION_TOKEN }}
      awdev-nuget-source: ${{ secrets.AWDEV_NUGET_URL }}
      npm-package-token: ${{ secrets.AWDEV_NPM_PACKAGE_TOKEN }}
      npmrc-config: ${{ secrets.NPMRC_CONFIG }}

  use-download:
    name: Use download
    needs: build
    runs-on: ubuntu-latest

    steps:
      - name: "Download Artifact"
        uses: actions/download-artifact@v2.0.8
        with:
          name: ${{ needs.build-windows.outputs.downloads }}
```

### Build macos

```yml
jobs:
  build:
    uses: actiwaredevelopment/action-io-build-workflows/.github/workflows/build-dotnet-io-module.yml@main
    with:
      repository: ${{ github.repository }}
      branch: ${{ github.ref_name }}
      dotnet-version: 6.0.x
      project: src/service-v2-dotnet/service/io2-module-template-service.csproj
      modulefile: "module.zip"
      dockerfile: "./.github/docker/dockerfile"
      ui-build-path: "./src/configuration/build"
      rid: "macos-x64"
      create-single-file: true
      module-artifact: "DesktopExtension" # Optional: downloads the given artifact, before creating module zip
      module-artifact-path: "./" # Optional: where to download the module-artifact
    secrets:
      action-user: ${{ secrets.GH_ACTION_USER }}
      action-token: ${{ secrets.GH_ACTION_TOKEN }}
      awdev-nuget-source: ${{ secrets.AWDEV_NUGET_URL }}
      npm-package-token: ${{ secrets.AWDEV_NPM_PACKAGE_TOKEN }}
      npmrc-config: ${{ secrets.NPMRC_CONFIG }}

  use-download:
    name: Use download
    needs: build
    runs-on: ubuntu-latest

    steps:
      - name: "Download Artifact"
        uses: actions/download-artifact@v2.0.8
        with:
          name: ${{ needs.build.outputs.downloads }}
```

## Build workflows for nodejs IO Modules

### Build

```yml
jobs:
  build:
    uses: actiwaredevelopment/action-io-build-workflows/.github/workflows/build-nodejs-io-module.yml@main
    with:
      repository: ${{ github.repository }}
      branch: ${{ github.ref_name }}
      project: src/service-v2-nodejs
      ui-project: src/configuration
      modulefile: "module.zip"
      dockerfile: "./.github/docker/dockerfile-nodejs"
      build-path: "./dist-nodejs"
      build-ui-path: "./dist"
      module-artifact: "DesktopExtension" # Optional: downloads the given artifact, before creating module zip
      module-artifact-path: "./" # Optional: where to download the module-artifact
    secrets:
      action-user: ${{ secrets.GH_ACTION_USER }}
      action-token: ${{ secrets.GH_ACTION_TOKEN }}
      awdev-nuget-source: ${{ secrets.AWDEV_NUGET_URL }}
      npm-package-token: ${{ secrets.AWDEV_NPM_PACKAGE_TOKEN }}
      npmrc-config: ${{ secrets.NPMRC_CONFIG }}

  use-download:
    name: Use download
    needs: build
    runs-on: ubuntu-latest

    steps:
      - name: "Download Artifact"
        uses: actions/download-artifact@v2.0.8
        with:
          name: ${{ needs.build.outputs.downloads }}
```

## Build workflows for golang IO Modules

### Build for linux

```yml
jobs:
  build:
    uses: actiwaredevelopment/action-io-build-workflows/.github/workflows/build-golang-io-module.yml@main
    with:
      repository: ${{ github.repository }}
      branch: ${{ github.ref_name }}
      mainfile: src/service-v2-go/cmd/service/main.go
      outputfile: io-module-service
      modulefile: "module.zip"
      dockerfile: "./.github/docker/dockerfile-go"
      go-os: "linux"
      go-arch: "amd64"
      module-artifact: "DesktopExtension" # Optional: downloads the given artifact, before creating module zip
      module-artifact-path: "./" # Optional: where to download the module-artifact
    secrets:
      action-user: ${{ secrets.GH_ACTION_USER }}
      action-token: ${{ secrets.GH_ACTION_TOKEN }}
      awdev-nuget-source: ${{ secrets.AWDEV_NUGET_URL }}
      npm-package-token: ${{ secrets.AWDEV_NPM_PACKAGE_TOKEN }}
      npmrc-config: ${{ secrets.NPMRC_CONFIG }}

  use-download:
    name: Use download
    needs: build
    runs-on: ubuntu-latest

    steps:
      - name: "Download Artifact"
        uses: actions/download-artifact@v2.0.8
        with:
          name: ${{ needs.build-nodejs.outputs.downloads }}
```

### Build for windows

```yml
jobs:
  build:
    uses: actiwaredevelopment/action-io-build-workflows/.github/workflows/build-golang-io-module.yml@main
    with:
      repository: ${{ github.repository }}
      branch: ${{ github.ref_name }}
      mainfile: src/service-v2-go/cmd/service/main.go
      outputfile: io-module-service
      modulefile: "module.zip"
      dockerfile: "./.github/docker/dockerfile-go"
      go-os: "windows"
      go-arch: "amd64"
      module-artifact: "DesktopExtension" # Optional: downloads the given artifact, before creating module zip
      module-artifact-path: "./" # Optional: where to download the module-artifact
    secrets:
      action-user: ${{ secrets.GH_ACTION_USER }}
      action-token: ${{ secrets.GH_ACTION_TOKEN }}
      awdev-nuget-source: ${{ secrets.AWDEV_NUGET_URL }}
      npm-package-token: ${{ secrets.AWDEV_NPM_PACKAGE_TOKEN }}
      npmrc-config: ${{ secrets.NPMRC_CONFIG }}

  use-download:
    name: Use download
    needs: build
    runs-on: ubuntu-latest

    steps:
      - name: "Download Artifact"
        uses: actions/download-artifact@v2.0.8
        with:
          name: ${{ needs.build-nodejs.outputs.downloads }}
```

### Build for macos (M1)

```yml
jobs:
  build:
    uses: actiwaredevelopment/action-io-build-workflows/.github/workflows/build-golang-io-module.yml@main
    with:
      repository: ${{ github.repository }}
      branch: ${{ github.ref_name }}
      mainfile: src/service-v2-go/cmd/service/main.go
      outputfile: io-module-service
      modulefile: "module.zip"
      dockerfile: "./.github/docker/dockerfile-go"
      go-os: "darwin"
      go-arch: "arm64"
      module-artifact: "DesktopExtension" # Optional: downloads the given artifact, before creating module zip
      module-artifact-path: "./" # Optional: where to download the module-artifact
    secrets:
      action-user: ${{ secrets.GH_ACTION_USER }}
      action-token: ${{ secrets.GH_ACTION_TOKEN }}
      awdev-nuget-source: ${{ secrets.AWDEV_NUGET_URL }}
      npm-package-token: ${{ secrets.AWDEV_NPM_PACKAGE_TOKEN }}
      npmrc-config: ${{ secrets.NPMRC_CONFIG }}

  use-download:
    name: Use download
    needs: build
    runs-on: ubuntu-latest

    steps:
      - name: "Download Artifact"
        uses: actions/download-artifact@v2.0.8
        with:
          name: ${{ needs.build-nodejs.outputs.downloads }}
```

## Publish docker images

### Publish to Docker Hub

```yml
jobs:
  build:
    uses: actiwaredevelopment/action-io-build-workflows/.github/workflows/build-dotnet-io-module.yml@main
    with:
      repository: ${{ github.repository }}
      branch: ${{ github.ref_name }}
      dotnet-version: 6.0.x
      project: src/service-v2-dotnet/service/io2-module-template-service.csproj
      module-source-path: "./module-definition"
      module-info-json: "./module-definition/info.json"
      modulefile: "module.zip"
      dockerfile: "./.github/docker/dockerfile"
      ui-build-path: "./src/configuration/build"
      rid: "win-x64"
      create-single-file: true
      module-artifact: "DesktopExtension" # Optional: downloads the given artifact, before creating module zip
      module-artifact-path: "./" # Optional: where to download the module-artifact
    secrets:
      action-user: ${{ secrets.GH_ACTION_USER }}
      action-token: ${{ secrets.GH_ACTION_TOKEN }}
      awdev-nuget-source: ${{ secrets.AWDEV_NUGET_URL }}
      npm-package-token: ${{ secrets.AWDEV_NPM_PACKAGE_TOKEN }}
      npmrc-config: ${{ secrets.NPMRC_CONFIG }}

  docker:
    needs: [build]

    uses: actiwaredevelopment/action-io-build-workflows/.github/workflows/publish-to-docker.yml@main
    with:
      artifact: ${{ needs.build.outputs.downloads }}
      image-name: "actiwareio/io-module-template"
      image-tag: 2-latest
    secrets:
      docker-username: ${{ secrets.DOCKER_HUB_USER }}
      docker-password: ${{ secrets.DOCKER_HUB_SECRET }}
```

### Publish to Github Docker

```yml
jobs:
  build:
    uses: actiwaredevelopment/action-io-build-workflows/.github/workflows/build-dotnet-io-module.yml@main
    with:
      repository: ${{ github.repository }}
      branch: ${{ github.ref_name }}
      dotnet-version: 6.0.x
      project: src/service-v2-dotnet/service/io2-module-template-service.csproj
      module-source-path: "./module-definition"
      module-info-json: "./module-definition/info.json"
      modulefile: "module.zip"
      dockerfile: "./.github/docker/dockerfile"
      ui-build-path: "./src/configuration/build"
      rid: "linux-x64"
      create-single-file: false
      module-artifact: "DesktopExtension" # Optional: downloads the given artifact, before creating module zip
      module-artifact-path: "./" # Optional: where to download the module-artifact
    secrets:
      action-user: ${{ secrets.GH_ACTION_USER }}
      action-token: ${{ secrets.GH_ACTION_TOKEN }}
      awdev-nuget-source: ${{ secrets.AWDEV_NUGET_URL }}
      npm-package-token: ${{ secrets.AWDEV_NPM_PACKAGE_TOKEN }}
      npmrc-config: ${{ secrets.NPMRC_CONFIG }}

  docker:
    needs: [build]

    uses: actiwaredevelopment/action-io-build-workflows/.github/workflows/publish-to-github-docker.yml@main
    with:
      artifact: ${{ needs.build.outputs.downloads }}
      image-name: io-module-template
      image-tag: developer
    secrets:
      token: ${{ secrets.GITHUB_TOKEN }}
```

### Publish to AWS

```yml
jobs:
  build:
    uses: actiwaredevelopment/action-io-build-workflows/.github/workflows/build-dotnet-io-module.yml@main
    with:
      repository: ${{ github.repository }}
      branch: ${{ github.ref_name }}
      dotnet-version: 6.0.x
      project: src/service-v2-dotnet/service/io2-module-template-service.csproj
      module-source-path: "./module-definition"
      module-info-json: "./module-definition/info.json"
      modulefile: "module.zip"
      dockerfile: "./.github/docker/dockerfile"
      ui-build-path: "./src/configuration/build"
      rid: "linux-x64"
      create-single-file: false
      module-artifact: "DesktopExtension" # Optional: downloads the given artifact, before creating module zip
      module-artifact-path: "./" # Optional: where to download the module-artifact
    secrets:
      action-user: ${{ secrets.GH_ACTION_USER }}
      action-token: ${{ secrets.GH_ACTION_TOKEN }}
      awdev-nuget-source: ${{ secrets.AWDEV_NUGET_URL }}
      npm-package-token: ${{ secrets.AWDEV_NPM_PACKAGE_TOKEN }}
      npmrc-config: ${{ secrets.NPMRC_CONFIG }}

  docker:
    needs: [build]

    uses: actiwaredevelopment/action-io-build-workflows/.github/workflows/publish-to-aws.yml@main
    with:
      artifact: ${{ needs.build.outputs.downloads }}
      region: eu-west-1
      image-name: io-module-template
      image-tag: developer
    secrets:
      access-key-id: ${{ secrets.AWS_ACCESS_KEY_ID }}
      secret-access-key: ${{ secrets.AWS_SECRET_ACCESS_KEY }}
```

### Publish to FTP

```yml
jobs:
  build:
    uses: actiwaredevelopment/action-io-build-workflows/.github/workflows/build-dotnet-io-module.yml@main
    with:
      repository: ${{ github.repository }}
      branch: ${{ github.ref_name }}
      dotnet-version: 6.0.x
      project: src/service-v2-dotnet/service/io2-module-template-service.csproj
      module-source-path: "./module-definition"
      module-info-json: "./module-definition/info.json"
      modulefile: "module.zip"
      dockerfile: "./.github/docker/dockerfile"
      ui-build-path: "./src/configuration/build"
      rid: "win-x64"
      create-single-file: true
    secrets:
      action-user: ${{ secrets.GH_ACTION_USER }}
      action-token: ${{ secrets.GH_ACTION_TOKEN }}
      awdev-nuget-source: ${{ secrets.AWDEV_NUGET_URL }}
      npm-package-token: ${{ secrets.AWDEV_NPM_PACKAGE_TOKEN }}
      npmrc-config: ${{ secrets.NPMRC_CONFIG }}

  ftp:
    needs: [build]

    uses: actiwaredevelopment/action-io-build-workflows/.github/workflows/publish-to-ftp.yml@main
    with:
      artifact: ${{ needs.build.outputs.download }}
      remote-dir: /Development/actiwareio/Source/Modules/v2/Template/
    secrets:
      address: ${{ secrets.SFTP_HOST }}:522
      user: ${{ secrets.SFTP_USER }}
      password: ${{ secrets.SFTP_PASSWORD }}
```
