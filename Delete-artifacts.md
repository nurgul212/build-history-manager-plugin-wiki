This page presents example how to use [DeleteArtifacts](https://github.com/jenkinsci/build-history-manager-plugin/blob/master/src/main/java/pl/damianszczepanik/jenkins/buildhistorymanager/model/actions/DeleteArtifactsAction.java) action:

# Requirements

# Configuration
```groovy
pipeline {
  agent any

  options {
    buildDiscarder(

        BuildHistoryManager(
            [
                continueAfterMatch: false,
                matchAtMost: 2
            ],
            [
                actions : [DeleteArtifacts()],
                continueAfterMatch: false,
                matchAtMost : 3
            ],
            [
                actions: [DeleteBuild()],
            ]
        )
    )
  }
}
```

# Example
