This page presents example how to use [BuildResult](https://github.com/jenkinsci/build-history-manager-plugin/blob/master/src/main/java/pl/damianszczepanik/jenkins/buildhistorymanager/model/conditions/BuildResultCondition.java) action:

# Requirements
- keep the 3 most recent build
- keep at least one build with successful result

# Configuration
```groovy
pipeline {
    agent any
    options {
        buildDiscarder(

            BuildHistoryManager([
                [
                    continueAfterMatch: false,
                    matchAtMost: 3
                ],
                [
                    conditions: [
                        BuildResult(matchSuccess: true)],
                    continueAfterMatch: false,
                    matchAtMost: 1
                ],
                [
                    actions: [DeleteBuild()]
                ]
            ])
            
        )
    }

    stages {
        stage('Demo') {
            steps {
                echo "Hello!"
            }
        }
    }
}
```

# Example

Following presents build history before and after running above configuration. Numbers refer to build number.

| Before | After | Reason |
|-|-|-|
| <div align="center">[`23`]<br>with artifact</div> | <div align="center">[`23`]<br>with artifact</div> | Matched by first rule.<br>No action performed. |
| <div align="center">[`24`]<br>without artifact</div> | <div align="center">[`24`]<br>without artifact</div> | Matched by first rule, no changes performed.<br>This is the last build performed by first rule because of `matchAtMost: 2` |
| <div align="center">[`25`]<br>with artifact</div> | <div align="center">[`25`]<br>without artifact</div> | Matched by second rule, artifact has been deleted |
| <div align="center">[`30`]<br>without artifact</div> | <div align="center">[`30`]<br>without artifact</div> | Matched by second rule.<br>Artifact should be removed but it is not published. |
| <div align="center">[`31`]<br>without artifact</div> | <div align="center">[`31`]<br>without artifact</div> | Matched by second rule.<br>Artifact should be removed but it was not present.<br>This is the last build performed by first rule because of `matchAtMost: 3` |
| <div align="center">[`32`]<br>with artifact</div> |  | Matched by second rule.<br>Build has been removed. |
| <div align="center">[`35`]<br>with artifact</div> |  | Matched by second rule.<br>Build has been removed. |
