This page presents example how to use [DeleteArtifactsWithPattern](https://github.com/jenkinsci/build-history-manager-plugin/blob/master/src/main/java/pl/damianszczepanik/jenkins/buildhistorymanager/model/actions/DeleteArtifactsWithPatternAction.java) action:

# Requirements
- In the configuration below, we simulate the date(age) using build number range instead of waiting for 5 days, 7 days or whatever. Assuming that we're simulating a job which runs daily after 7 runs, 7 days should have elapsed.
- Delete all artifacts except the logs after 2 days
- Delete all artifacts after 5 days.

# Configuration
```groovy
final int buildNumber = Integer.parseInt(env.BUILD_NUMBER, 10)
properties(
    [
        buildDiscarder(BuildHistoryManager(
                [
                    [
                        actions: [DeleteArtifactsWithPattern(exclude: '**/*.log', include: '**')],
                        conditions: [BuildNumberRange(maxBuildNumber: buildNumber - 2, minBuildNumber: 0)]
                    ],
                    [
                        actions: [DeleteArtifactsWithPattern(exclude: '', include: '**')],
                        conditions: [BuildNumberRange(maxBuildNumber: buildNumber - 5, minBuildNumber: 0)]
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

| Run | Days Ago | Expected Artifacts|
|-|-|-|
| <div align="center">[`1`]</div> | <div align="center">`6`</div> | All artifacts were deleted. |
| <div align="center">[`2`]</div> | <div align="center">`5`</div> | All artifacts were deleted. |
| <div align="center">[`3`]</div> | <div align="center">`4`</div> | `testFolder/testLog1.log, testLog.log` |
| <div align="center">[`4`]</div> | <div align="center">`3`</div> | `testFolder/testLog1.log, testLog.log` |
| <div align="center">[`5`]</div> | <div align="center">`2`</div> | `testFolder/testLog1.log, testLog.log` |
| <div align="center">[`6`]</div> | <div align="center">`1`</div> | `test.xml, testFolder/test1.xml, testFolder/testLog1.log, testFolder/testTxt1.txt, testLog.log, testTxt.txt` |
| <div align="center">[`7`]</div> | <div align="center">`0`</div> | `test.xml, testFolder/test1.xml, testFolder/testLog1.log, testFolder/testTxt1.txt, testLog.log, testTxt.txt` |
