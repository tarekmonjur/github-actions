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
Github use compute resources to running each jobs called `runner`, To do that need to use `run-on` attribute.
```
jobs:
  job1:
    name: First Job
    run-on: ubuntu-latest
  job2:
    name: Second Job
    run-on: window-latest
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

### Example:
```
name: First

on: push

jobs:
  job1:
    name: First Job
    run-on: ubuntu-latest
    steps:
      - name: Step One
        uses: actions/checkout@v3
      - name: Step 
        run: env | sort

  job2:
    name: Second Job
    run-on: windows-latest
    steps:
      - name: Step One
        uses: actions/checkout@v3
      - name: Step Two
        run: "Get-ChildTiem Env: | Sort-Object Name"
```

