Creates an AOSP mirror of a more reasonable size than standard as it doesn't include outdated projects.

## Quick Start

```sh
# eg. For creating a mirror that only has Android 8.0.0 and newer with no device projects:
$ mkdir -p /usr/local/aosp/mirror
$ cd /usr/local/aosp/mirror
$ repo init -u https://github.com/BlackRamLabs/android-mirror-manifest -b since_8.0.0_no_device_projects --mirror
$ repo sync

# eg. For working on _master_ branch:
$ mkdir -p /usr/local/aosp/master
$ cd /usr/local/aosp/master
$ repo init -u /usr/local/aosp/mirror/platform/manifest.git
$ repo sync

# eg. When the mirror manifest has beeen updated and you would like the new projects (to support the latest master code):
# Update the mirror
$ cd /usr/local/aosp/mirror
$ repo sync
# Update the project
$ cd /usr/local/aosp/master
$ repo sync
```

## Overview

The mirror manifest can be used to create a local mirror of official Android source repos using the following commands

```sh
$ mkdir -p /usr/local/aosp/mirror
$ cd /usr/local/aosp/mirror
$ repo init -u https://android.googlesource.com/mirror/manifest --mirror
$ repo sync
```

The local mirror can then be used like this

```sh
$ mkdir -p /usr/local/aosp/master
$ cd /usr/local/aosp/master
$ repo init -u /usr/local/aosp/mirror/platform/manifest.git
$ repo sync
```

The [official mirror manifest](https://android.googlesource.com/mirror/manifest) contains all the projects that have been part
of some Android release, even if they were removed in newer releases. This makes a complete mirror unnecessarily huge.

The downsized manifest file [default.xml](https://github.com/BlackRamLabs/android-mirror-manifest/blob/master/default.xml) only contains
projects used in **Android Oreo** (Android 8) and newer versions of Android, with no device projects. It was generated using the Python program
[prunemirrormanifest.py](https://github.com/BlackRamLabs/android-mirror-manifest/blob/master/prunemirrormanifest.py) like this
```sh
$ python3 prunemirrormanifest.py
The following Android releases are available.

['2.2.3', '2.3.6', '2.3.7', '4.0.1', '4.0.2', '4.0.3', '4.0.4', '4.1.1', '4.1.2', '4.3.1', '4.4.1', '4.4.2', '4.4.3', '4.4.4', '5.0.0', '5.0.1', '5.0.2', '5.1.0', '5.1.1', '6.0.0', '6.0.1', '7.0.0', '7.1.0', '7.1.1', '7.1.2', '8.0.0', '8.1.0', '9.0.0']
Enter the oldest desired release : 8.0.0

Prune device projects? (y/N): y
```

To use this smaller mirror manifest just replace the URL _https://android.googlesource.com/mirror/manifest_ with _https://github.com/BlackRamLabs/android-mirror-manifest_
in the standard mirroring steps.

If you also want to build Android releases older than Oreo, then fork this repo, clone your fork locally, run _prunemirrormanifest.py_ and choose an older release, push your updates, and init a repo mirror pointing at _https://github.com/YOUR_USER/android-mirror-manifest_
