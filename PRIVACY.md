# JTV privacy policy

Last updated: 15 October 2026

JTV is a self-hosted media server and a companion player. This policy describes how the JTV product handles information.

## Who operates JTV

You (or your household) run the JTV **server** on your own computer, typically Ubuntu with Docker. There is no required JTV cloud account.

The **player app** (coming to Google Play soon) is a client. You type the address of *your* server. The app then signs in and plays from that server.

## What the player collects

The player stores on the device:

- The server URL you enter
- A sign-in token so you stay logged in

It does **not** include streaming subscriptions, and it does not need access to your Google account beyond installing the app from Play.

Playback, favorites, continue watching, and user accounts live on **your server**, not on a JTV-operated website.

## What the server stores

On the machine you control, JTV stores:

- Usernames and password hashes
- Per-user provider credentials you assign in the Dashboard
- Favorites and continue-watching progress
- Cached artwork and guide data used to show libraries

That data stays in your server’s database volume unless you back it up or copy it yourself.

## Network

The player and browser talk to the server URL you configured (default port **18096** on the LAN). JTV does not require a third-party analytics SDK for the product described here.

If you expose JTV on the public internet, you are responsible for HTTPS, firewall rules, and who can reach it.

## Children

JTV is a media client for a server the operator already runs. It is not directed at children under 13. Parents who install it control the server and the accounts on it.

## Changes

If this policy changes, the date at the top of this file will be updated.

## Contact

Use the contact details on the JTV Google Play listing once the player is published, or the operator of your own JTV server.
