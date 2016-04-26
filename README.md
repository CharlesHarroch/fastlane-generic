My Fastlane configuration is inspired to Touchwonders Fastfile configuration.

Fastlane Configuration documentation
================
# Installation
```
sudo gem install fastlane
```
# Architecture

## Remote Fastfile

In my Mobile department we decided to implement a base collection of standard lanes and actions that we want to share among all our projects.
To import them we take advantage of [import_from_git](https://github.com/KrauseFx/fastlane/blob/master/docs/Advanced.md#import_from_git) action.

__For Custom lane :__

At very beginning of every project's Fastfile:
```
fastlane init
emacs fastlane/Fastfile
import_from_git(url:"git@github.com:CharlesHarroch/fastlane-generic.git}:/library/fastlane.git", path:"Fastfile")
```

__For Generic lane :__

In yout project directory :
```
git clone git@github.com:CharlesHarroch/fastlane-generic.git
```

## Configuration file

Every project's fastlane folder contains a required configuration file responsible to load proper environment and settings per project deployment.

Example: [fastlane_config.yaml](fastlane_config.yaml)

# Available actions

## iOS & Android
### Load release notes

Load release notes for current project's deployment.

The build note file content is loaded after the last tag and finally used for fill in Crashlytics and Appetize.io build's release notes.

### GIT Checkout

Checkout a branch from project working copy.

# Availabe lanes

The predefined lanes are divided in _public_ and _private_.

The rule of thumb is to keep the public lanes as simple as possible by leveraging all the implementation load to private lanes such that they can be easily re-used.

## Public lanes

### Development
# iOS : OK
# Android : OK

__Description__: Submit a new daily build to Crashlytics.

__Steps__:

* Increment build number
* Download latest provisioning profile for targets specified in `fastlane_configuration.yaml` file.
* Add icon overlay if needed (see `fastlane_configuration.yml`)
* Publish to Crashlytics. By default all team members  is invited.

### Live Preview
# iOS : OK
# Android : Comming Soon !!

__Description__: Submit a new build to Apptize.io for test application directly from your browser

* Download latest provisioning profile for targets specified in `fastlane_configuration.yaml` file.
* Build application with sdk specified in `fastlane_configuration.yaml` file.
* Publish to Appetize.io.

### Testing
# iOS : OK
# Android : Comming Soon !!
__Description__: Start Units test & User interface test

* Build application for testing
* Start testUI & Units test
* Generate an HTML report

### Stable
# iOS : OK
# Android : OK
__Description__: Submit a new sprint-end/weekly build to Crashlytics & Appetize.io.

__Steps__:

* Increment build number
* Download latest provisioning profile for targets specified in `fastlane_configuration.yaml` file.
* Add icon overlay if needed (see `fastlane_configuration.yaml`)
* Publish to Crashlytics. By default all team members is invited.

### Release
# iOS : Comming Soon !!
# Android : Comming Soon !!

__Description__: Submit a new app version to AppStore.

__Steps__:

* Download latest provisioning profile for targets specified in `fastlane_configuration.yaml` file.
* Upload to iTunes Connect.

## Private lanes

### `config_env`

__Description__: Configure the environment to drive required lanes with provided options.

__Steps__:

* Select proper Xcode tool by looking up the `xcode_select` parameter defined in project’s `fastlane_configuration.yaml` file.

### `prepare`

__Description__: Prepare required tools to drive required lanes with provided options.

__Step__:

* Ensure clean `git status`.
* Checkout/Pull latest and greatest from `remote_branch` parameter specified in project’s `fastlane_configuration.yaml` file.
* Cocoapods.

### `get_provisioning_profile`

__Description__: Create/Renew/Download required provisioning profiles for all targets specified in `fastlane_configuration.yaml` file.

__Step__:

* Download targets provisioning profile. Every targets provides an identifier which is used to create an environment variable containing proper mobile provisioning profile UDID. This is useful in case the UDID is fetched from an environment variable, in your Xcode project’s setting.

### `project_warm_up`

__Description__: Last step before ‘build&archive’ process.

__Step__:

* Add icon overlay if needed.
* Increment build number.
N.B.: Release lane will __never__ increment project build number.

### `build_and_archive`

__Description__: Build and export IPA for your project. All intermediate and final step result are named after project’s scheme defined in the `fastlane_configuration.yaml`

__Step__:

* Build project by using parameters specified in `fastlane_configuration.yaml` file.
* Export build’s product to IPA.

## `publish`

__Description__: Publish build to Crashlytics by using build notes available from last tag.

__Step__:

* Load build release notes from last tag.
* Publish build to Crashlytics. When specified, emails have prevalence over groups notification.

----
More information about fastlane can be found on [https://fastlane.tools](https://fastlane.tools).
The documentation of fastlane can be found on [GitHub](https://github.com/KrauseFx/fastlane)
