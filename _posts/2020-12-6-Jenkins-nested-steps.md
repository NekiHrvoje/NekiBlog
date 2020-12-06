---
title: 'Jenkins user inputs'
description: 'Jenkins user input with nested stages.'
date: '2020-02-02'
modified_date: '2020-02-02'
image: '/assets/images/posts/greneral-jenkins.png'
---

Making user inputs in Jenkins is pretty much straight forward,
until you realise jobs are queuing  up because you blocked the executor.

To remedy that we need to assign agent none to that part of the pipeline,
so we don't allocate any resources.

We can do that in a few ways:
  1. expert way - by changing agents in pipeline and stashing/unstashing files
  2. right way - by using nodes and killing agents
  3. retarded way - by using completely separate  pipeline running on agent none
  4. lazy way - by using nested steps

Being lazy here is the number four.
Jenkins supports defining agent on stage level and stage supports nested stages.
Basically you can have stages/stage/stages/stage/steps... :)
Point here is that we can define agent none for input step and use a different agent
for all other steps. 

Jenkinsfile example:

```js
pipeline {

  agent none

  stages {
    stage ('Approval for producion') {
            steps {
                timeout(time: 3, unit: "HOURS") {
                input message: "Do you approve deployment?", ok: 'Yes'
              	}
            } 
    }
    stage ("Deploy") {
      agent any
      stages {
        stage('SCM') {
            steps {
              //SCM commands
            }
        }
        stage('Deploy app') {
            steps {
              //Deploy commands
            } 
        }
      }
    }
  }
}
```
