# Overview

Component extensions (or, more simply, components) allow Brave to receive asynchronous updates to core functionality without needing to update Brave itself. Currently, Brave uses components to download the data files associated with Ad Block, HTTPS Everywhere, Tracking Protection, Tor Client Updater and Widevine Content Decryption(Windows and macOS) and Crypto Wallets.

# Installation and Updates

On startup, Brave installs its registered components via `https://laptop-updates.brave.com/extensions`. If already installed, Brave verifies that the components are up-to-date. Once installed/verified, Brave checks for new updates every 5 hours.

# Performing an On-Demand Update Check

To perform an on-demand update check of your installed components, visit `brave://components/` in Brave and press the appropriate "Check for update" button.