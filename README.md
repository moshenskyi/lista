
# *lista* - CLI tool for activity listing

Lists all activities for the app by specified package. If multiple devices are connected it **autocompletes** serial ID of all available devices

The main purpose is to use it for your own or company projects. If you need to find and fix the bug and don't know where to start because your project is too big and has multiple [Activities](https://developer.android.com/guide/components/activities/intro-activities)

Table of Contents
=================

   * [Obfuscated apps](#obfuscated-apps)
   * [Demo](#demo)
		* [Old approach](#old-approach)
        * [New approach](#new-approach)
   * [Usage](#usage)
   * [Examples](#examples)
   * [Installation](#installation)

## Obfuscated apps

If you use it for an obfuscated app (your prod build or someone else's app) you might end up with such weird names:

<img width="823" alt="Screenshot 2023-12-29 at 03 48 28" src="https://github.com/moshenskyi/lista/assets/18194203/ba482fac-8242-44ce-934d-522ce6bf44b5">

## Demo

The main problem with writing a chain of commands to achieve the same result is:
1. It's big and hard to remember
2. You always need to run an additional command to find a list of connected devices (if there is more than 1)
3. No output if nothing found

I propose a handy version with only 1 required option and out-of-the-box autocompletion to choose between available devices:

![lista-demo](https://github.com/moshenskyi/lista/assets/18194203/5ca728fa-229e-4ef9-8e0a-c4efef9c41f9)

Compare this chain of commands with lista:

#### Old approach:

    ❯ adb devices
    List of devices attached
    78f2ba2b	device

    ❯ adb -s 78f2ba2b shell dumpsys activity activities \
	    | grep com.huawei.smarthome | grep Hist \
	    | awk -F '[{}]' '{print $2}' | cut -d ' ' -f3
	    
    com.huawei.smarthome/.activity.MainActivity


#### New approach:
    ❯ lista -p com.huawei.smarthome -s 78f2ba2b

    Serial ID: 78f2ba2b
    Package name: com.huawei.smarthome
    
    Activity stack:
    com.huawei.smarthome/.activity.MainActivity
    com.huawei.smarthome/.login.LauncherActivity

As you can see, less typing, more info

## Usage

    lista [options]

    -s serial id - device serial ID. Autocompletable field.
	It should be used if more than one Android device is connected to your machine.
    
    -p package name (required unless -h is used). 
    Example: -p "com.google.mail"
    
    -h get help

## Examples

    # If only one device is attached
    lista -p "com.google.mail"

    # If multiple devices are connected, add serial ID with the -s option. Autocompletable field
    lista -p "com.google.mail" -s 78f2ba2b
    
    # Help menu
    lista -h


## Installation

The only prerequisite is to have `adb` installed. Usually, it's shipped with `Android Studio`

- If you have `Android Studio` installed, you just need to add `adb`  to the `$PATH` variable ([guide](https://medium.com/macoclock/add-adb-to-path-on-osx-8bc2f11a19ea))

- If not, here is a guide on how to install it - [link](https://stackoverflow.com/a/32314718/7805359)

You can just download, copy, or clone the code and one of the approaches would be to move this script to `/usr/local/bin/` folder.

It's a user scripts folder that can be found in `$PATH`.
