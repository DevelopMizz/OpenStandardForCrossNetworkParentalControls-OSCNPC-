 - As part of the setup routine for a device, it MUST provide a Yes/No prompt to enable parental controls.
 - The default policy should be `"DNSChildSafeBlocklist": 2 (moderate)`.
Devices that want to be compliant MUST also implament a GUI to set the `LimitInternetTime` time options, and should present rgw UI
when the user selects `Yes` to enable parental controls. 
