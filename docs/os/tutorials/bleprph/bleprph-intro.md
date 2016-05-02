## BLE Peripheral Project

### Introduction

#### Overview

*bleprph* is an example app included in the apache-mynewt-core repository.  This app implements a simple BLE peripheral with the following properties:

* Supports three services: GAP, GATT, and alert notification service (ANS).
* Supports a single concurrent connection.
* Automatically advertises connectability when not connected to a central device.

This tutorial aims to provide a guided tour through the *bleprph* app source
code.  This document builds on some concepts described elsewhere in the Apache
Mynewt documentation.  Before proceding with this tutorial, you might want to
familiarize yourself with the following pages:

* [Create Your First Mynewt Project](../get_started/project_create/)
* [NimBLE Stack Initialization](../../network/ble/ini_stack/ble_ini_intro/)

#### Services, Characteristics, Descriptors

A BLE peripheral interfaces with other BLE devices by exposing *services*,
*characteristics*, and *descriptors*.  All three of these entities are
implemented at a layer layer via *attributes*.  If you are not familiar with
these concepts, you will probably want to check out this
[overview](https://developer.bluetooth.org/TechnologyOverview/Pages/GATT.aspx)
from the Bluetooth Developer's site before proceeding.

Now let's dig in to some C code.