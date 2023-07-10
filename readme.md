# Github Actions

## Simple Workflow

```
name: CI

on:
  push:
    branches: [ "master" ]

jobs:
  say-hello:
    runs-on: ubuntu-latest

    steps:
      - uses: actions/checkout@v3

      - name: Run a one-line script
        run: echo Hello, world!

      - name: Run a multi-line script
        run: |
          echo Add other actions to build,
          echo test, and deploy your project.
```

### **Name Attribute**
 * The name of the workflow
 * Its not required

### **On Attribute**
 * The Github event that trigers the workflow
 * Its required

### **On Events**
 * Repository events
 * pust
 * pull_request
 * release
 * Webhooks
    - Branch Creation
    - Issues
    - Members
 * Scheduled

### **Jobs Attribute**
 Its defines a block that lists the jobs for the workflow.
 * Workflow must have at least one job.
 * Each job must have a unique identifier.
 * Job run in parallel by default.
 * Must start with later or underscore.
 * Can only contain alphanumeric characters, -, _

### **Runs On Attribute**
 This let github which type of machine we do like to use to run the jobs.
 * The type of machine needed to run jobs.
 * This environment are known as `runners`.
 * Github also allow self-hosted runners.

### **Steps Attribute**
Action are bundle of code used to perform a specific task or operation and essentially actions are docker images.

 * List of actions or commands.
 * Access to the file system.
 * Each step runs in its own process.

- ### Name Attribute
    * An optional identifier for the step.

- ### Uses Attribute
    * Identifies an action to use.
    * Define the location of that action.

- ### Run Attribute
    * Its runs commands in the virtual environment shell.

<br>

## Workflows
Doing anyting useful with an action, we need to start with a workflow.

* Workflows define the github event that tirggers exection of any actions.
* Workflow also define which actions to run based on the event.
* One repositorries can contain multiple workflows.

### Workflow Events:
[Github action workflow events doc.](https://docs.github.com/en/actions/using-workflows/events-that-trigger-workflows#push)

**Define single event**
```
On: push
```
**Define multiple event**
```
On: [push, release]
```

### Workflow Jobs:
Github use compute resources to running each jobs called `runner`, To do that need to use `runs-on` attribute.
```
jobs:
  job1:
    name: First Job
    runs-on: ubuntu-latest
  job2:
    name: Second Job
    runs-on: window-latest
```

### Workflow Steps:
* Steps are tasks within a job.
* Steps run as processes on the compute resource.
* Steps can run commands or actions.

**Run:** Execute an command in the operating system shell's.

Single Line Command:
```
steps:
  - name: Run a command
    run: echo Hello, world!
```

Multiline Line Command:
```
steps:
  - name: Run a command
    run: |
      command 1
      command 2
```

**Uses:** Execute an action in the operating system.
```
steps:
  - name: Run a actiion
    uses: actions/checkout@v3
```

**Workflow Example:**
```
name: Second Workflow

on:
  push:
    branches:
      - '*'
      - '!master'

jobs:
  job1:
    name: First Job
    runs-on: ubuntu-latest
    steps:
      - name: Step One
        uses: actions/checkout@v3
      - name: Step 
        run: env | sort

  job2:
    name: Second Job
    runs-on: windows-latest
    steps:
      - name: Step One
        uses: actions/checkout@v3
      - name: Step Two
        run: "Get-ChildTiem Env: | Sort-Object Name"
```

### Workflow Job Dependency:
When one job output need for another job input

**Needs Attribute:**

Identifies one or more jobs that must complete successfully before a job will run.
```
needs: [job1, job2]
```

### Workflow Conditions:
we can limiting workflow activity useing branch condition.
```
on
  push:
    branches:
      - '*'
      - '!master'
  pull_request:
    branches:
      - master
```

**Workflow Example**
```
name: Third Workflow

on:
  push:
    brances:
      - 'test2'
      - '!master'

jobs:
  job1:
    name: First Job
    runs-on: ubuntu-latest
    steps:
      - name: Step One
        uses: actions/checkout@v3
      - name: Step 
        run: env | sort

  job2:
    name: Second Job
    runs-on: windows-latest
    steps:
      - name: Step One
        uses: actions/checkout@v3
      - name: Step Two
        run: "Get-ChildTiem Env: | Sort-Object Name"

  job3:
    name: Third Job
    runs-on: ubuntu-latest
    needs: [job1, job2]
    steps:
      - name: Step One
        run: echo "dependency example"
```

<br>

## Actions:
Action can be define from three locations.
### Action Location:
**Public Repository:**
```
uses: {owner}/{repo}@{ref}
uses: actions/checkout@v3
```
**The same repo as workflow:**
```
uses: ./path/to/the/action
uses: ./.github/actions/my-local-action
```
**A docker image registry**
```
uses: docker://{image}:{tag}
uses: docker://hello-world:latest
```

### Action Arguments:
we can passing arguments to an action by using with keyword.
```
uses: actions/checkout@v3
with:
  repository: tarekmonjur/javascript
  ref: master
  path: ./example
```
**Example Action Arguments**
```
name: Challenge One

on:
  push:
    branches:
      - 'master'

jobs:
  build:
    runs-on: ubuntu-latest
    steps:
      - name: Checkout Project
        uses: actions/checkout@v3

      - name: Show Project files and node version
        run: |
          ls -la
          node -v
          npm -v

      - name: Setup NodeJs
        uses: actions/setup-node@v3
        with:
          node-version: '16.16.0'
          architecture: x64

      - name: Checkout nodejs example
        uses: actions/checkout@v3
        with:
          repository: tarekmonjur/javascript
          ref: master
          path: ./example

      - name: Show files and node version
        run: |
          ls -la
          node -v
          npm -v

      - name: Run Game Example
        run: |
          ls
          cd example/example
          ls
          node game1.js
```

### Environment Variables:
Variable is read from the shell, Shell Variable Syntext
```
$VARIABLE_NAME
```
Variable is read from the workflow.
```
${{env.VARIABLE_NAME}}
```

### Artifacts:
Artifacts using to keep something after a workflow or job has been completed.

* Data preserved from a workflow
* Files or collectioin of files
  - Compiled binaries
  - Archives
  - Test results
  - Logs files
* Pass data between workflow jobs
* Can only be uploaded by a workflow
  - actions/upload-artifact
* Can only be downloaded by the uploading workflow
  - actions/download-artifact

**ENV and Artifacts Example:**
```
name: artifact

on: [push]

env:
  FILE_NAME: hello-server

jobs:
  build:
    name: Build
    runs-on: ubuntu-latest
    steps:

    - name: Check out code
      uses: actions/checkout@v1

    - name: Build ${{ env.FILE_NAME }} for ubuntu-latest
      run: go build ${{ env.FILE_NAME }}.go
      
    - name: Upload artifact for linux
      uses: actions/upload-artifact@v1.0.0
      with:
        name: linux
        path: ./${{ env.FILE_NAME }}

  test-linux:
    name: Test Linux
    runs-on: [ubuntu-latest]
    needs: [build]
    steps:

    - name: Check out code
      uses: actions/checkout@v1

    - name: Download artifact
      uses: actions/download-artifact@v1.0.0
      with:
        name: linux

    - name: Test ${{ env.FILE_NAME }}
      run: source ./test.sh

```

<br>

## Manual Trigger:
To enable a workflow to be triggered manually, you need to configure the `workflow_dispatch` event.

```
name: Manual trigger

on:
  workflow_dispatch:
    inputs:
      name:
        description: "Who to greet"
        required: true
        type: string
        default: "World"
        type: choice
        options:
        - prod
        - uat
        - dev
      environment:
        description: 'Environment to run tests against'
        type: environmen
jobs:
    hello:
      runs-on: ubuntu-latest

      steps:
      - name: Hello Step
        run: echo "Hello ${{ github.event.inputs.name }}"   
```

<br>

# Advance Workflow

## Service Container:
Service container are docker containers that run as a part of job.
There are a few thing that have to consider when using service container.

* Job runner must be linux based.
* Self-Hosted runners must use linux and docker.
* Services will be run directly on the runner.
* Use localhost and a mapped port to connect to the service.

```
env:
  DATABASE_NAME: test

job:
  build:
    runs-on: ubuntu-latest
    services:
      mysqldb:
        image: mysql
        env:
          MYSQL_PASSWORD: root
          MYSQL_DB: ${{ env.DATABASE_NAME }}
        ports:
          - 3307:3306
        volumes:
          - /soruce/directory:/destination/directory
        options: >-
          --health-interval 10s
          --health-timeout 5s
          --health-retries 5      
```

## Scheduled Trigger:
Its use to run workflow on the specific time. Its usefull for nightly builds. Generating monthly report and or any other automation that needs to be run on a schedule.

* Schedule use UTC (coordinated unversal time) time.

```
name: scheduled workflow

on:
  workflow_dispatch:
  schedule:
    - cron: '*/10 * * * *'
```

## Composite Action:
Composite action use for prevent duplicate steps using again and again on the workflows. Its like a create a function then use it multiple places.

```
name:
description:
inputs:
outputs:
runs:
  using: "composite"
  steps:
    - run:
    - uses:
```

## Environment:
Create a environment from github settings paage for manual approvals and deployent.
```
jobs:
  production:
    runs-on: ubuntu-latest
    environment: prod
    needs: development
    steps:
```

## Caching:
Caching make workflow faster.
[See this example.](https://github.com/automate6500/01-06-caching)

## Matrix Strategy:
Run workflow within multiple platforms and versions.

```
jobs:
  build:
    strategy:
      matrix:
        version: [14, 16, 18]
        platform: [ubuntu-latest, macos-latest, windows-latest]
    runs-on: ${{ matrix.platform }}
    steps:
      - uses: actions/setup-node@v3
        with:
          node-version: ${{ matrix.version }}
```
### Matrix - include and exclude option:
```
jobs:
  build:
    strategy:
      matrix:
        version: [14, 16, 18]
        platform: [ubuntu-latest, macos-latest, windows-latest]
        include:
          - platform: ubuntu-latest
            version: 17
        exclude:
          - platform: windows-latest
            version: 14    
    runs-on: ${{ matrix.platform }}
    steps:
      - uses: actions/setup-node@v3
        with:
          node-version: ${{ matrix.version }}
```

### Matrix Strategy Options
* `fail-fast` option default value is true
* `max-parallel` option
* `continue-on-error` option
```
jobs:
  build:
    strategy:
      max-parallel: 3
      fail-fast: false
      matrix:
        version: [14, 16, 17]
        platform: [ubuntu-latest, macos-latest, windows-latest]
        experimental: [false]
        include:
          - platform: ubuntu-latest
            version: 18
            experimental: true
        exclude:
          - platform: windows-latest
            version: 14
    continue-on-error: ${{ matrix.experimental }}  
    runs-on: ${{ matrix.platform }}
    steps:
      - uses: actions/setup-node@v3
        with:
          node-version: ${{ matrix.version }}      
```