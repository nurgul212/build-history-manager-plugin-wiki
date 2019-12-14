This article provides best practice about how to write good rule to avoid problems.

# Testing mode
Never, ever test your rules for production builds. Especially if any of your action deletes builds or artifacts. This is because if you make mistake you will not have possibility to recover build which was deleted accidentally.
For testing purpose it is recommended to have separate build and to see the log file where information about executing rules, conditions and actions are stored.

# Perform faster
For separable conditions it is worth to remember about stopping processing second rule when first matches. This should allow to perform processing faster especially if build is not removed and might be evaluated unnecessary during each build.

# Parallel builds
Some condition might not work properly for not completed builds. For that cases result is not available or artifacts might not be published yet.
That is the reason why plugin [iterates over](https://javadoc.jenkins.io/hudson/model/Run.html#getPreviousCompletedBuild--) only completed builds. If you have situation when last started build can be completed earlier then previous one then plan your rules carefully and mind that sometimes the most recent builds may not look like you have planned to be.