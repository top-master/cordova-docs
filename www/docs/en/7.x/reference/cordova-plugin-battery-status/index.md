---
edit_link: 'https://github.com/apache/cordova-plugin-battery-status/blob/master/README.md'
title: Battery Status
plugin_name: cordova-plugin-battery-status
plugin_version: master
description: Get events for device battery level.
---

<!-- WARNING: This file is generated. See fetch_docs.js. -->

<!--
# license: Licensed to the Apache Software Foundation (ASF) under one
#         or more contributor license agreements.  See the NOTICE file
#         distributed with this work for additional information
#         regarding copyright ownership.  The ASF licenses this file
#         to you under the Apache License, Version 2.0 (the
#         "License"); you may not use this file except in compliance
#         with the License.  You may obtain a copy of the License at
#
#           http://www.apache.org/licenses/LICENSE-2.0
#
#         Unless required by applicable law or agreed to in writing,
#         software distributed under the License is distributed on an
#         "AS IS" BASIS, WITHOUT WARRANTIES OR CONDITIONS OF ANY
#         KIND, either express or implied.  See the License for the
#         specific language governing permissions and limitations
#         under the License.
-->

|Android 4.4|Android 5.1|Android 6.0|iOS 9.3|iOS 10.0|Windows 10 Store|Travis CI|
|:-:|:-:|:-:|:-:|:-:|:-:|:-:|
|[![Build Status](http://cordova-ci.cloudapp.net:8080/buildStatus/icon?job=cordova-periodic-build/PLATFORM=android-4.4,PLUGIN=cordova-plugin-battery-status)](http://cordova-ci.cloudapp.net:8080/job/cordova-periodic-build/PLATFORM=android-4.4,PLUGIN=cordova-plugin-battery-status/)|[![Build Status](http://cordova-ci.cloudapp.net:8080/buildStatus/icon?job=cordova-periodic-build/PLATFORM=android-5.1,PLUGIN=cordova-plugin-battery-status)](http://cordova-ci.cloudapp.net:8080/job/cordova-periodic-build/PLATFORM=android-5.1,PLUGIN=cordova-plugin-battery-status/)|[![Build Status](http://cordova-ci.cloudapp.net:8080/buildStatus/icon?job=cordova-periodic-build/PLATFORM=android-6.0,PLUGIN=cordova-plugin-battery-status)](http://cordova-ci.cloudapp.net:8080/job/cordova-periodic-build/PLATFORM=android-6.0,PLUGIN=cordova-plugin-battery-status/)|[![Build Status](http://cordova-ci.cloudapp.net:8080/buildStatus/icon?job=cordova-periodic-build/PLATFORM=ios-9.3,PLUGIN=cordova-plugin-battery-status)](http://cordova-ci.cloudapp.net:8080/job/cordova-periodic-build/PLATFORM=ios-9.3,PLUGIN=cordova-plugin-battery-status/)|[![Build Status](http://cordova-ci.cloudapp.net:8080/buildStatus/icon?job=cordova-periodic-build/PLATFORM=ios-10.0,PLUGIN=cordova-plugin-battery-status)](http://cordova-ci.cloudapp.net:8080/job/cordova-periodic-build/PLATFORM=ios-10.0,PLUGIN=cordova-plugin-battery-status/)|[![Build Status](http://cordova-ci.cloudapp.net:8080/buildStatus/icon?job=cordova-periodic-build/PLATFORM=windows-10-store,PLUGIN=cordova-plugin-battery-status)](http://cordova-ci.cloudapp.net:8080/job/cordova-periodic-build/PLATFORM=windows-10-store,PLUGIN=cordova-plugin-battery-status/)|[![Build Status](https://travis-ci.org/apache/cordova-plugin-battery-status.svg?branch=master)](https://travis-ci.org/apache/cordova-plugin-battery-status)|

# cordova-plugin-battery-status

This plugin provides an implementation of an old version of the [Battery Status Events API][w3c_spec]. It adds the following three events to the `window` object:

* batterystatus
* batterycritical
* batterylow

Applications may use `window.addEventListener` to attach an event listener for any of the above events after the `deviceready` event fires.

## Installation

    cordova plugin add cordova-plugin-battery-status

## Status object

All events in this plugin return an object with the following properties:

- __level__: The battery charge percentage (0-100). _(Number)_
- __isPlugged__: A boolean that indicates whether the device is plugged in. _(Boolean)_

## batterystatus event

Fires when the battery charge percentage changes by at least 1 percent, or when the device is plugged in or unplugged. Returns an [object][status_object] containing battery status.

### Example

    window.addEventListener("batterystatus", onBatteryStatus, false);

    function onBatteryStatus(status) {
        console.log("Level: " + status.level + " isPlugged: " + status.isPlugged);
    }

### Supported Platforms

- Amazon Fire OS
- iOS
- Android
- BlackBerry 10
- Windows Phone 7 and 8
- Windows (Windows Phone 8.1 and Windows 10)
- Firefox OS
- Browser (Chrome, Firefox, Opera)

### Quirks: Android &amp; Amazon Fire OS

**Warning**: the Android and Fire OS implementations are greedy and prolonged use will drain the device's battery.

### Quirks: Windows Phone 7 &amp; Windows Phone 8

The `level` property is _not_ supported on Windows Phone 7 because the OS does not provide native APIs to determine battery level. The `isPlugged` parameter _is_ supported.

### Quirks: Windows Phone 8.1

The `isPlugged` parameter is _not_ supported on Windows Phone 8.1. The `level` parameter _is_ supported.

## batterylow event

Fires when the battery charge percentage reaches the low charge threshold. This threshold value is device-specific. Returns an [object][status_object] containing battery status.

### Example

    window.addEventListener("batterylow", onBatteryLow, false);

    function onBatteryLow(status) {
        alert("Battery Level Low " + status.level + "%");
    }

### Supported Platforms

- Amazon Fire OS
- iOS
- Android
- BlackBerry 10
- Firefox OS
- Windows (Windows Phone 8.1 and Windows 10)
- Browser (Chrome, Firefox, Opera)

### Quirks: Windows Phone 8.1

The `batterylow` event fires on Windows Phone 8.1 irrespective of whether the device is plugged in or not. This happens because the OS does not provide an API to detect whether the device is plugged in.

## batterycritical event

Fires when the battery charge percentage reaches the critical charge threshold. This threshold value is device-specific. Returns an [object][status_object] containing battery status.

### Example

    window.addEventListener("batterycritical", onBatteryCritical, false);

    function onBatteryCritical(status) {
        alert("Battery Level Critical " + status.level + "%\nRecharge Soon!");
    }

### Supported Platforms

- Amazon Fire OS
- iOS
- Android
- BlackBerry 10
- Firefox OS
- Windows (Windows Phone 8.1 and Windows 10)
- Browser (Chrome, Firefox, Opera)

### Quirks: Windows Phone 8.1

The `batterycritical` event fires on Windows Phone 8.1 irrespective of whether the device is plugged in or not. This happens because the OS does not provide an API to detect whether the device is plugged in.

[w3c_spec]: http://www.w3.org/TR/2011/WD-battery-status-20110915/
[status_object]: #status-object
