# Introduction
We have an internal Microsoft executive review coming up in September for Thali. Even though this is internal we try to be very transparent about what's going on with Thali. So we will track the work we are doing for it here. Our main goal is to only do work that actually directly contributes to Thali reaching V1. In that past we have done one off hacks to make something work for a demo and always ended up regretting the wasted time.

# Requirements

Demonstrate ability to:
* reach peers in hostile network environments
* securely pair peers
* securely communicate between peers
* robustly synch data between peers
* easily write a program that leverages the previous infrastructure

# Technical solutions to requirements

* Robust Tor hidden service support
 * For bonus points we would also support wi-fi direct, at least in Android but this is a pri2.
* Address book to manage relationships + QRCode support
 * For bonus points we would also support email
* Mutual SSL Auth support
* CouchDB Replication both using CouchBase and using PouchDB
* HTML5 development environment 

# Proposed Demo
1. We would start by demo'ing the address book with QR Code support (and email support if available) to show how people can exchange identities (this will also boot strap our environment for next step)
1. We would write an instant messaging client 'live', we would would then build the apk and distZip for desktop and copy it to a FTP location.
1. We would have multiple desktops and devices in the room which would use the FTP to pull down the app and run it on their machines
1. We would then have folks enter chat messages and show them showing up on all the other devices
1. If, by some miracle, we have wi-fi direct support (say just on Android) then we would demo turning off corporate Wi-Fi and showing us using wi-fi direct
1. We would then end with PPNet showing us forming rooms, exchanging photos, etc. The point of bringing in PPNet is show how nice things can look at what kind of full functionality one can have.

# IM Demo Details

The demo would be a single global chat. Anyone in your address book can play. The program flow would be something like:

1. Draw the screen which would have a single chat window and a line to enter text
1. The program would create (if it doesn't already exist) a database named 'chatdemo' both in PouchDB and in the local TDH.
1. The program would set up a timer to do continuous one-off replications between PouchDB's 'chatdemo' and the TDH's 'chatdemo' (we should probably provide our own library for this since this is a work around for problems with continuous replication)
1. The program would set up a replication command to the TDH to do repeated one offs between the TDH and everyone in the user's address book (flooding). Note that we will provide this library either as a Javascript library or as the first implementation of the replication manager.
1. We would then listen to the changes feed for the chatdemo DB in PouchDB. This is just a stream of JSON. Every time we sync a new update from someone we will (or should anyway, need to test) get a change notification that we can automatically just paste directly into the chat window.
1. If the user types text then we would post it to the 'chatdemo' database and count on the sync code to both trigger a notification (we need to test if that is true)