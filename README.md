# GitHub Action: Run command over SSH

A simple GitHub Action to connect to a remote server over SSH, and run a command.

## Example

```yaml
name: Deploy to staging server
on:
  push:
    branches:
      - main

  workflow_dispatch:

jobs:
  deploy:
    name: Deploy to staging server
    runs-on: ubuntu-latest

    steps:
      - name: Deploy to staging server
        uses: Ayesh/ssh-run-command@v1
        id: ssh_exec
        with:
          ssh_key: ${{ secrets.SSH_PRIVATE_KEY }}
          ssh_port: 22
          ssh_username: root
          ssh_host: 1.2.3.4
          command_cwd: "/root/work"
          command: sh build/build.sh $GITHUB_REF_NAME

      - name: Show outputs
        uses: Ayesh/ssh-run-command@v1
        run: |
          echo "Command exit was: ${{ steps.ssh_exec.outputs.exit_code }}"
          echo "Output"
          echo "${{ steps.ssh_exec.outputs.output }}"
```

## Inputs:

|name| Required | Default | Description                                         |
|----|:--------:|---------|-----------------------------------------------------|
|`command`|Yes|_none_|The command to run after connecting to the server and `cd` into the `command_cwd`|
|`command_cwd`|    No    | `.`     | A directory to `cd` into before running the command |
|`ssh_host`|   Yes    | _none_  | SSH connection host without port                    |
|`ssh_port`|    No    | `22`    | SSH connection port number                          |
|`ssh_username`|    No    | `root`  | SSH connection username                             |
|`ssh_key`|   Yes    | _none_  | SSH private key contents                            |

## Outputs

|Name| Description                           |
|----|---------------------------------------|
|`exit_code`| Exit code of the command run over SSH |
|`output`| The output from command run over SSH  |
