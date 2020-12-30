# Workspace Demos

## bundleChecksumMD5

In version `3.4.0` of `com.liferay.gradle.plugins.workspace` Gradle plugin, we 
  added a new feature for verify bundle checksums.  There is a new property on the
  `gradle.liferayWorkspace` extension object called `bundleChecksumMD5`  

If there is a `gradle.liferayWorkspace.bundlURL` property set, either in script,
  or from the `gradle.properties` `liferay.workspace.bundle.url` property, then
  the workspace plugin will calculate a `default` value for this property based
  on the value of the bundleURL in the following manner.

```
bundleChecksumMD5Default = bundleURL + ".MD5"
```

If either the "default" MD5 url returns a MD5 file or the user manually sets
  the bundleChecksumMD5 property like this:

```
gradle.liferayWorkspace {
  bundleChecksumMD5 = "qwertyasdfjkl"
}
```

So in this case you will likely want to set both bundleURL and bundleChecksumMD5
  at the same time.

```
gradle.liferayWorkspace {
  bundleURL = "https:///url/to/custom_bundle.zip"
  bundleChecksumMD5 = "560236ecaa1400c256abb3b0be18de03"
}
```

If there is a valid MD5 file found (either from default value or from a custom
  setting) then workspace will configure a task called `verifyBundle`.  This 
  task will depend on `downloadBundle` and also `initBundle` will depend on 
  `verifyBundle` task.  So if the md5 doesn't match, then the verifyBundle task 
  will fail like this:
  
```
> Task :downloadBundle
> Task :verifyBundle FAILED

FAILURE: Build failed with an exception.

* What went wrong:
Execution failed for task ':verifyBundle'.
> Invalid checksum for file 'liferay-dxp-tomcat-7.3.10-ga1-20200930160533946.tar.gz'. Expected invalid12345 but got 5c04ca5099ff3dff395ea96f53ff36f5.
```