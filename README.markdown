# Workspace Demos

## bundleChecksumMD5

In version `3.4.3` of `com.liferay.gradle.plugins.workspace` Gradle plugin, we 
  added a new feature for verify bundle checksums.  There is a new property on the
  `gradle.liferayWorkspace` extension object called `bundleChecksumMD5`  

  It can be set using `gradle.properties` called `liferay.workspace.bundle.checksum.md5`
  
If this property has been set either in the properties file or via a script using
the `gradle.liferayWorkspace.bundleChecksumMD5` property, then the verifyBundle
task will be enabled and will verify the download file with the MD5.

If you use the `product` key field, `liferay.workspace.product=dxp-7.3-ga1` or
any other product key, the value of the `bundle.checksum.md5` will be automatically
supplied and the `verifyBundle` task will be enabled.

So if you specify a custom bundle.url, you will need to supply your own custom 
md5 checksum value.

If the `verifyBundle` task is enabled and the MD5 value of the downloaded file 
doesn't match, you will see the following error:

```
> Task :downloadBundle
> Task :verifyBundle FAILED

FAILURE: Build failed with an exception.

* What went wrong:
Execution failed for task ':verifyBundle'.
> Invalid checksum for file 'liferay-dxp-tomcat-7.3.10-ga1-20200930160533946.tar.gz'. Expected invalid12345 but got 5c04ca5099ff3dff395ea96f53ff36f5.
```