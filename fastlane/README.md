fastlane documentation
================
# Installation

Make sure you have the latest version of the Xcode command line tools installed:

```
xcode-select --install
```

Install _fastlane_ using
```
[sudo] gem install fastlane -NV
```
or alternatively using `brew cask install fastlane`

# Available Actions
### tests
```
fastlane tests
```


----

## iOS
### ios build
```
fastlane ios build
```
This lane generated the build
### ios generate_build_name
```
fastlane ios generate_build_name
```

### ios generate_build
```
fastlane ios generate_build
```
This lane will build application with the provided configuration
### ios lint
```
fastlane ios lint
```

### ios analysis
```
fastlane ios analysis
```

### ios upload
```
fastlane ios upload
```
This lane will upload build to the respective distribution opted for

----

This README.md is auto-generated and will be re-generated every time [fastlane](https://fastlane.tools) is run.
More information about fastlane can be found on [fastlane.tools](https://fastlane.tools).
The documentation of fastlane can be found on [docs.fastlane.tools](https://docs.fastlane.tools).
