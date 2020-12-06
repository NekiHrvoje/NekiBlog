---
title: 'Jenkins user inputs'
description: 'Jenkins user inputs without blocking resources.'
date: '2020-02-02'
modified_date: '2020-02-02'
image: '/assets/images/posts/greneral-jenkins.png'
---

svasta nesto sadrzajno.

Example with image:

![Error](@@baseUrl@@/assets/images/posts/error.png)

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
