This page presents example how to use [BuildNumberRange](https://github.com/jenkinsci/build-history-manager-plugin/blob/master/src/main/java/pl/damianszczepanik/jenkins/buildhistorymanager/model/conditions/BuildNumberRangeCondition.java) condition:

# Requirements
- delete build with build number less or equal to 70
- delete build with build number between 73~75, both included

# Configuration
```groovy
pipeline {
    agent any
    options {
        buildDiscarder(

            BuildHistoryManager([
                [
                    conditions: [
                        BuildNumberRange(maxBuildNumber: 75, minBuildNumber: 73)],
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
| <div align="center">[`80`]</div> | <div align="center">[`80`]</div> | None rule matched. |
| <div align="center">[`74`]</div> | | Matched by first rule.<br>Build has been removed. |
| <div align="center">[`73`]</div> | | Matched by first rule.<br>Build has been removed. |
| <div align="center">[`71`]</div> | <div align="center">[`71`]</div> | None rule matched. |
| <div align="center">[`70`]</div> | | Matched by second rule.<br>Build has been removed. |
| <div align="center">[`2`]</div> | <div align="center">[`2`]</div> | None rule matched. |
| <div align="center">[`1`]</div> | <div align="center">[`1`]</div> | None rule matched. |
