You can check the list of components installed in Brave Browser by navigating to brave://components:

## Components supported by Brave

1. **Brave Local Data Updater**: Used to update AutoplayWhitelist, ExtensionWhitelist, Greaselion scripts and ReferrerWhitelist.
2. **Brave Ad Block Updater (Base + Regional)**: Updates the ad-block lists regularly
3. **Brave User Model Installer (en)**: 
4. **NTP Sponsored Images (US)**:
5. **Crowd Deny**:
6. **CRLSet**:
7. **Safety Tips**:
8. **Brave SpeedReader Updater**:
9. **File Type Policies**:
10. **Brave User Model Installer (US)**:
11. **Brave HTTPS Everywhere Updater**:

## Components not supported by Brave:

1. **Legacy TLS Deprecation Configuration**: This component adds a whitelist of domains that can use deprecated TLS 1.0/1.1 components.
2. **Federated Learning of Cohorts**: This component
3. **Zxcvbn Data Dictionaries**: This component includes a list of patterns that can be used to determine the password strength. Brave has disabled the password check feature in brave and will be working on a custom version. Work is being tracked here: https://github.com/brave/brave-browser/issues/12001
4. 

## In discussion:

1. **MEI Preload**: Currently supported. We should disable it to prevent media being preloaded by Brave.
2. 