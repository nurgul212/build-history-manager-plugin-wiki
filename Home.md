Following page describes available conditions and actions that are supported by the plugin. Documentation is same as available during plugin configuration, implementation shows the code what it does and how. Finally examples contain pipeline configuration ready to use (copy&paste) with some sample build history that shows its state before and after plugin execution.
If the feature you are looking for is missing, file the issue or pull request.
Before starting to test new configuration, read [some tips](https://github.com/jenkinsci/build-history-manager-plugin/wiki/Building-good-rules)

# Conditions
Describes available conditions

## BuildResultCondition
- [Documentation](https://github.com/jenkinsci/build-history-manager-plugin/blob/master/src/main/resources/pl/damianszczepanik/jenkins/buildhistorymanager/model/conditions/BuildResultCondition/help.html)
- [Implementation](https://github.com/jenkinsci/build-history-manager-plugin/blob/master/src/main/java/pl/damianszczepanik/jenkins/buildhistorymanager/model/conditions/BuildResultCondition.java)
- [Example](https://github.com/jenkinsci/build-history-manager-plugin/wiki/Build-result-condition)

## BuildNumberRangeCondition
- [Documentation](https://github.com/jenkinsci/build-history-manager-plugin/blob/master/src/main/resources/pl/damianszczepanik/jenkins/buildhistorymanager/model/conditions/BuildNumberRangeCondition/help.html)
- [Implementation](https://github.com/jenkinsci/build-history-manager-plugin/blob/master/src/main/java/pl/damianszczepanik/jenkins/buildhistorymanager/model/conditions/BuildNumberRangeCondition.java)

## BuildAgeRangeCondition
- [Documentation](https://github.com/jenkinsci/build-history-manager-plugin/blob/master/src/main/resources/pl/damianszczepanik/jenkins/buildhistorymanager/model/conditions/BuildAgeRangeCondition/help.html)
- [Implementation](https://github.com/jenkinsci/build-history-manager-plugin/blob/master/src/main/java/pl/damianszczepanik/jenkins/buildhistorymanager/model/conditions/BuildAgeRangeCondition.java)

## TokenMacroCondition 
- [Documentation](https://github.com/jenkinsci/build-history-manager-plugin/blob/master/src/main/resources/pl/damianszczepanik/jenkins/buildhistorymanager/model/conditions/TokenMacroCondition/help.html)
- [Implementation](https://github.com/jenkinsci/build-history-manager-plugin/blob/master/src/main/java/pl/damianszczepanik/jenkins/buildhistorymanager/model/conditions/TokenMacroCondition.java)

## MatchEveryBuildCondition
- [Documentation](https://github.com/jenkinsci/build-history-manager-plugin/blob/master/src/main/resources/pl/damianszczepanik/jenkins/buildhistorymanager/model/conditions/MatchEveryBuildCondition/help.html)
- [Implementation](https://github.com/jenkinsci/build-history-manager-plugin/blob/master/src/main/java/pl/damianszczepanik/jenkins/buildhistorymanager/model/conditions/MatchEveryBuildCondition.java)


# Actions
Describes available actions
## DeleteArtifactsAction
- [Documentation](https://github.com/jenkinsci/build-history-manager-plugin/blob/master/src/main/resources/pl/damianszczepanik/jenkins/buildhistorymanager/model/actions/DeleteArtifactsAction/help.html)
- [Implementation](https://github.com/jenkinsci/build-history-manager-plugin/blob/master/src/main/java/pl/damianszczepanik/jenkins/buildhistorymanager/model/actions/DeleteArtifactsAction.java)
- [Example](https://github.com/jenkinsci/build-history-manager-plugin/wiki/Delete-artifacts-action)

## DeleteBuildAction
- [Documentation](https://github.com/jenkinsci/build-history-manager-plugin/blob/master/src/main/resources/pl/damianszczepanik/jenkins/buildhistorymanager/model/actions/DeleteBuildAction/help.html)
- [Implementation](https://github.com/jenkinsci/build-history-manager-plugin/blob/master/src/main/java/pl/damianszczepanik/jenkins/buildhistorymanager/model/actions/DeleteBuildAction.java)

# Use cases
Several cases with configuration that can be reused to solve concrete problems
## Simple

## Complex
