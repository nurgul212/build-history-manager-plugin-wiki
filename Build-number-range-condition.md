This page presents example how to use [BuildNumberRange](https://github.com/jenkinsci/build-history-manager-plugin/blob/master/src/main/java/pl/damianszczepanik/jenkins/buildhistorymanager/model/conditions/BuildNumberRangeCondition.java) condition:

# Requirements
- delete build with build number greater or equal to 70
- delete build with build number between 73~75 included

# Configuration
```groovy
pipeline {
    agent any
    options {
        buildDiscarder(

            BuildHistoryManager([
                [
                    conditions: [ BuildNumberRange(maxBuildNumber: 75, minBuildNumber: 73)],
                    actions: [ DeleteBuild() ]
                ],
                [
                    conditions: [
                        BuildNumberRange(maxBuildNumber: 70)],
                    actions: [ DeleteBuild() ]
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

Following presents build history before and after running above configuration. Numbers refer to Jenkins job number.

| Before | After | Reason |
|-|-|-|
| <div align="center">[`80`]</div> | <div align="center">[`23`]</div> | Matched by first rule.<br>None action performed. |
| <div align="center">[`75`]</div> | <div align="center">[`24`]</div> | Matched by first rule.<br> None changes performed.<br>This is the last build matched by this rule because of `matchAtMost: 2` |
| <div align="center">[`74`]</div> | <div align="center">[`25`]</div> | Matched by second rule, artifact has been deleted |
| <div align="center">[`73`]</div> | <div align="center">[`30`]</div> | Matched by second rule.<br>Artifact should be removed but it is not published. |
| <div align="center">[`70`]</div> | <div align="center">[`31`]</div> | Matched by second rule.<br>Artifact should be removed but it was not present.<br>This is the last build matched by this rule because of `matchAtMost: 3` |
| <div align="center">[`2`]</div> |  | Matched by second rule.<br>Build has been removed. |
| <div align="center">[`1`]</div> |  | Matched by second rule.<br>Build has been removed. |
