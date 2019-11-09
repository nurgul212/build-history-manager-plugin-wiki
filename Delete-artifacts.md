This page presents example how to use [DeleteArtifacts](https://github.com/jenkinsci/build-history-manager-plugin/blob/master/src/main/java/pl/damianszczepanik/jenkins/buildhistorymanager/model/actions/DeleteArtifactsAction.java) action:

# Requirements
I want to have:
- at most two the most recent build saved
- three more should be saved as well but without artifacts
- all the rest must be removed forever

# Configuration
```groovy
pipeline {
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
