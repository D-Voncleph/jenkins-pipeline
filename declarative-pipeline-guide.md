# Understanding Declarative Pipelines

**Date:** February 7, 2026  
**Project:** FinTech DevOps - Phase 3

## 1. What is a Declarative Pipeline?

It is a structured way of defining a Jenkins pipeline using a simplified, opinionated syntax. Unlike "Scripted Pipelines" (which are pure Groovy code), Declarative Pipelines use a strict block-based structure that is easier to read and maintain.

## 2. The Anatomy of a `Jenkinsfile`

Every declarative pipeline follows this specific hierarchy:

```groovy
pipeline {
    agent any
    stages {
        stage('Name') {
            steps {
                // Commands go here
            }
        }
    }
}
```

## Key Components

### A. `pipeline { ... }`
* **Function:** The absolute outer wrapper.
* **Rule:** Everything must happen inside these brackets. It tells Jenkins "This is a Declarative Pipeline, not a Scripted one."

### B. `agent`
* **Function:** Defines WHERE the code runs.
* **Common Options:**
  * `agent any`: Run on the first available executor (Master or Slave).
  * `agent none`: Do not allocate a node globally (useful if specific stages need different nodes).
  * `agent { docker 'python:3.9' }`: Run inside a specific Docker container.

### C. `stages { ... }`
* **Function:** A container for the sequence of events.
* **Rule:** You can only have one `stages` block in the main pipeline.

### D. `stage('Name') { ... }`
* **Function:** A logical unit of work (e.g., "Build", "Test", "Deploy").
* **Visualization:** Each `stage` creates a distinct column in the Jenkins UI "Stage View."

### E. `steps { ... }`
* **Function:** The actual commands to execute.
* **Examples:**
  * `sh 'npm install'`: Run a shell command (Linux).
  * `echo 'Hello'`: Print to the logs.
  * `git url: '...'`: Checkout specific code.

## 3. Example Workflow

(From our Multi-Stage Project)

```groovy
stages {
    stage('Cloning Code') {
        steps {
            sh 'ls -la' // Verifies the workspace
        }
    }
    stage('Running Tests') {
        steps {
            sh 'echo "Tests Passed"' // Simulates a test suite
        }
    }
}
```

---

*Save the file.*
