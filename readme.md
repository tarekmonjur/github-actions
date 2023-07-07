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
 * Each job must have an identifier.
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