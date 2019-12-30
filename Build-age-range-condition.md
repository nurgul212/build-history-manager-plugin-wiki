This page presents example how to use [BuildAgeRange](https://github.com/jenkinsci/build-history-manager-plugin/blob/master/src/main/java/pl/damianszczepanik/jenkins/buildhistorymanager/model/conditions/BuildAgeRangeCondition.java) condition:

# Requirements
- delete artifacts for builds which were built yesterday or the day before yesterday

# Configuration
```groovy
pipeline {
    agent any
    options {
        buildDiscarder(

            BuildHistoryManager([
                [
                    conditions: [
                        BuildAgeRange(minDaysAge: 1, maxDaysAge: 2),
                    actions: [ DeleteArtifacts() ]
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
| <div align="center">[`80`]<br>today</div> | <div align="center">[`80`]</div> | None rule matched. |
| <div align="center">[`76`]<br>today</div> | <div align="center">[`76`]</div> | None rule matched. |
| <div align="center">[`75`]<br>yesterday</div> | | Matched by first rule.<br>Artifacts have been removed. |
| <div align="center">[`73`]<br>2 days old</div> | | Matched by first rule.<br>Artifacts have been removed. |
| <div align="center">[`71`]<br>4 days old</div> | <div align="center">[`71`]</div> | None rule matched. |

