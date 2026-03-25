# openmp-build-action
Compile your gamemode directly from GitHub.

# 📜 How to use
Within your SAMP project, create the following directory in the root folder: ".github/workflows" and inside workflows, create a .yml file with the name you want, for example "openmp-build.yml".
Now enter the configuration below.
```yml
name: Build and Deploy Gamemode

on:
  workflow_dispatch:

jobs:
  build:
    runs-on: windows-latest
    
    permissions:
      contents: write

    steps:
      - name: Checkout code
        uses: actions/checkout@v4

      - name: Build Gamemode using Custom Action
        uses: devbluen/openmp-build-action@v2
        with:
          gamemode_name: 'main'
          core_libs: 'amxsscanf libmariadb log-core'
          extra_data_folders: 'scriptfiles/database'

      - name: Upload Windows Artifact
        uses: actions/upload-artifact@v4
        with:
          name: build-windows
          path: deploy_win/

      - name: Upload Linux Standard Artifact
        uses: actions/upload-artifact@v4
        with:
          name: build-linux-standard
          path: deploy_linux/

      - name: Upload Linux Dynamic Artifact
        uses: actions/upload-artifact@v4
        with:
          name: build-linux-dynamic
          path: deploy_linux_dyn/
```

In steps, you can add some settings to get the best performance.

# ⚙️ Configurations

- `gamemode_name: 'main'`
#### This is extremely important because you are telling the compiler which gamemode it will compile.

- `omp_version: 'v1.5.8.3079'`
#### By default we use version 'v1.5.8.3079', but you can choose another version of your preference that comes directly from the open-mp repository.

- `core_libs: 'amxsscanf libmariadb log-core'`
#### The core_libs directory is responsible for copying certain resources that specific plugins require to run, such as MySQL which requires log_core. Therefore, you define everything here; by default, we already check and copy "amxsscanf libmariadb log-core".

- `core_config: 'mysql.ini config.json'`
#### The core_config command copies read-only files such as .ini, .json, or another file of your choice. By default, we check and copy the mysql.ini and config.json files.

- `extra_data_folders: 'scriptfiles/database scriptfiles/animations'`
#### This parameter allows you to copy an entire folder, such as scriptfiles/animations; it will copy everything inside that folder.
