This page presents an example how to use the [DeleteArtifactsMatchingPatterns](https://github.com/jenkinsci/build-history-manager-plugin/blob/master/src/main/java/pl/damianszczepanik/jenkins/buildhistorymanager/model/actions/DeleteArtifactsMatchingPatternsAction.java) action:

# Requirements
- Simulate a job which runs daily after 7 runs, 7 days should have elapsed.
- Delete all artifacts except the logs after 2 days.
- Delete all artifacts after 5 days.

# Configuration
```groovy
properties(
    [
        buildDiscarder(BuildHistoryManager(
                [
                    [
                        actions: [DeleteArtifactsMatchingPatterns(excludePatterns: '**/*.log', includePatterns: '**')],
                        conditions: [BuildAgeRange(maxDaysAge: 7, minDaysAge: 2)]
                    ],
                    [
                        actions: [DeleteArtifactsMatchingPatterns(excludePatterns: '', includePatterns: '**')],
                        conditions: [BuildAgeRange(maxDaysAge: 7, minDaysAge: 5)]
                    ]
                ]
            )
        )
    ]
)

node {

        sh '''
        rm -rf *
        touch test.xml
        touch testLog.log
        touch testTxt.txt
        mkdir -p testFolder
        touch testFolder/test1.xml
        touch testFolder/testLog1.log
        touch testFolder/testTxt1.txt
        '''
        archiveArtifacts artifacts: '**', followSymlinks: false
}
```

# Example

Following presents build history before and after running above configuration.

| Run# | Days Ago | Expected Artifacts| Reason |
|-|-|-|-|
| <div align="center">[`7`]</div> | <div align="center">`0`</div> | `test.xml, testFolder/test1.xml, testFolder/testLog1.log, testFolder/testTxt1.txt, testLog.log, testTxt.txt` | None rule matched.|
| <div align="center">[`6`]</div> | <div align="center">`1`</div> | `test.xml, testFolder/test1.xml, testFolder/testLog1.log, testFolder/testTxt1.txt, testLog.log, testTxt.txt` | None rule matched.|
| <div align="center">[`5`]</div> | <div align="center">`2`</div> | `testFolder/testLog1.log, testLog.log` | Matched by first rule.<br> All artifacts were deleted except the log files.|
| <div align="center">[`4`]</div> | <div align="center">`3`</div> | `testFolder/testLog1.log, testLog.log` | Matched by first rule.<br> All artifacts were deleted except the log files.|
| <div align="center">[`3`]</div> | <div align="center">`4`</div> | `testFolder/testLog1.log, testLog.log` | Matched by first rule.<br> All artifacts were deleted except the log files.|
| <div align="center">[`2`]</div> | <div align="center">`5`</div> |  | Matched by second rule.<br> All artifacts were deleted.|
| <div align="center">[`1`]</div> | <div align="center">`6`</div> |  | Matched by second rule.<br> All artifacts were deleted.|