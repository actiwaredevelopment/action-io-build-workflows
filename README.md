# ACTION: IO Build Workflows
In this repository workflows are offered, which can be used to build the projects or services of ACTIWARE.IO.

## Build workflows for .net IO Modules
### Build windows
```yml
jobs:
    build-windows:
      uses: actiwaredevelopment/action-io-build-workflows/.github/workflows/build-dotnet-io-module.yml@main
      with:
        repository: ${{ github.repository }}
        branch: ${{ github.ref_name }}
        dotnet-version: 6.0.x
        project: src/service-v2/service/io2-module-template-service.csproj
        modulefile: 'module.zip'
        dockerfile: './.github/docker/dockerfile'
        ui-build-path: './src/configuration/build'
        rid: 'win-x64'
        create-single-file: true
      secrets:
        action-user: ${{ secrets.GH_ACTION_USER }}
        action-token: ${{ secrets.GH_ACTION_TOKEN }}
        awdev-nuget-source: ${{ secrets.AWDEV_NUGET_URL }}
        npm-package-token: ${{ secrets.AWDEV_NPM_PACKAGE_TOKEN }}
        npmrc-config: ${{ secrets.NPMRC_CONFIG }}

    use-download:
      name: Use download
      needs: build-windows
      runs-on: ubuntu-latest

      steps:
      - name: 'Download Artifact'
        uses: actions/download-artifact@v2.0.8
        with: 
          name: ${{ needs.build-windows.outputs.downloads }}
```

### Build linux
```yml
jobs:
    build-linux:
      uses: actiwaredevelopment/action-io-build-workflows/.github/workflows/build-dotnet-io-module.yml@main
      with:
        repository: ${{ github.repository }}
        branch: ${{ github.ref_name }}
        dotnet-version: 6.0.x
        project: src/service-v2/service/io2-module-template-service.csproj
        modulefile: 'module.zip'
        dockerfile: './.github/docker/dockerfile'
        ui-build-path: './src/configuration/build'
        rid: 'linux-x64'
        create-single-file: false
      secrets:
        action-user: ${{ secrets.GH_ACTION_USER }}
        action-token: ${{ secrets.GH_ACTION_TOKEN }}
        awdev-nuget-source: ${{ secrets.AWDEV_NUGET_URL }}
        npm-package-token: ${{ secrets.AWDEV_NPM_PACKAGE_TOKEN }}
        npmrc-config: ${{ secrets.NPMRC_CONFIG }}

    use-download:
      name: Use download
      needs: build-windows
      runs-on: ubuntu-latest

      steps:
      - name: 'Download Artifact'
        uses: actions/download-artifact@v2.0.8
        with: 
          name: ${{ needs.build-windows.outputs.downloads }}
```

### Build macos
```yml
jobs:
    build-macos:
      uses: actiwaredevelopment/action-io-build-workflows/.github/workflows/build-dotnet-io-module.yml@main
      with:
        repository: ${{ github.repository }}
        branch: ${{ github.ref_name }}
        dotnet-version: 6.0.x
        project: src/service-v2/service/io2-module-template-service.csproj
        modulefile: 'module.zip'
        dockerfile: './.github/docker/dockerfile'
        ui-build-path: './src/configuration/build'
        rid: 'macos-x64'
        create-single-file: true
      secrets:
        action-user: ${{ secrets.GH_ACTION_USER }}
        action-token: ${{ secrets.GH_ACTION_TOKEN }}
        awdev-nuget-source: ${{ secrets.AWDEV_NUGET_URL }}
        npm-package-token: ${{ secrets.AWDEV_NPM_PACKAGE_TOKEN }}
        npmrc-config: ${{ secrets.NPMRC_CONFIG }}

    use-download:
      name: Use download
      needs: build-windows
      runs-on: ubuntu-latest

      steps:
      - name: 'Download Artifact'
        uses: actions/download-artifact@v2.0.8
        with: 
          name: ${{ needs.build-windows.outputs.downloads }}
```

## Publish docker images
### Publish to Docker Hub
```yml
jobs:
    build-windows:
      uses: actiwaredevelopment/action-io-build-workflows/.github/workflows/build-dotnet-io-module.yml@main
      with:
        repository: ${{ github.repository }}
        branch: ${{ github.ref_name }}
        dotnet-version: 6.0.x
        project: src/service-v2/service/io2-module-template-service.csproj
        module-source-path: './module-definition'
        module-info-json: './module-definition/info.json'
        modulefile: 'module.zip'
        dockerfile: './.github/docker/dockerfile'
        ui-build-path: './src/configuration/build'
        rid: 'win-x64'
        create-single-file: true
      secrets:
        action-user: ${{ secrets.GH_ACTION_USER }}
        action-token: ${{ secrets.GH_ACTION_TOKEN }}
        awdev-nuget-source: ${{ secrets.AWDEV_NUGET_URL }}
        npm-package-token: ${{ secrets.AWDEV_NPM_PACKAGE_TOKEN }}
        npmrc-config: ${{ secrets.NPMRC_CONFIG }}

    docker:
      uses: actiwaredevelopment/action-io-build-workflows/.github/workflows/publish-to-docker.yml@main
      with:
        artifact: ${{ needs.build-windows.outputs.downloads }}
        image-name: 'actiwareio/io-module-template'
        image-tag: 2-latest
      secrets:
        docker-username: ${{ secrets.DOCKER_HUB_USER }}
        docker-password: ${{ secrets.DOCKER_HUB_SECRET }}
```