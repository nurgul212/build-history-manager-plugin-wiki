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
                    conditions: [ BuildResult(matchSuccess: true) ],
                    continueAfterMatch: false,
                    matchAtMost: 1
                ],
                [
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
| <div align="center">[`56`]<br>success</div> | <div align="center">[`56`]<br>success</div> | Matched by first rule.<br>None action performed. |
| <div align="center">[`54`]<br>failure</div> | <div align="center">[`54`]<br>failure</div> | Matched by first rule.<br>None changes performed. |
| <div align="center">[`52`]<br>success</div> | <div align="center">[`52`]<br>success</div> | Matched by first rule.<br> None changes performed.<br>This is the last build matched by this rule because of `matchAtMost: 3` |
| <div align="center">[`50`]<br>failure</div> |  | Matched by third rule as the second rule does not meet condition.<br>Build has been removed. |
| <div align="center">[`41`]<br>failure</div> |  | Matched by third rule as the second rule does not meet condition.<br>Build has been removed. |
| <div align="center">[`35`]<br>success</div> | <div align="center">[`35`]<br>success</div> | Matched by second rule.<br>None changes performed.<br>This is the last build matched by this rule because of `matchAtMost: 1` |
| <div align="center">[`32`]<br>unstable</div> |  | Matched by third rule as the second rule does not meet condition.<br>Build has been removed. |
| <div align="center">[`30`]<br>success</div> |  | Matched by third rule.<br>Build has been removed. |
