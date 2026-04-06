 - As part of the setup routine for a device, it MUST provide a Yes/No prompt to enable parental controls.
 - The default policy should be `"DNSChildSafeBlocklist": 2 (moderate)`.
Devices that want to be compliant MUST also implament a GUI to set the `LimitInternetTime` time options, and should present rgw UI
when the user selects `Yes` to enable parental controls. 
- When a parental control enabled device connects to a network it should do send a `HTTP GET` to `/ParentalControls/OSCNPC` on the default gateway on `4080/TCP`.
NOTE 4080 should be replaced with an IANA registered port before real world adoption.
- The device should then send `HTTP POST` on `/ParentalControls/OSCNPC` with the settings payload to implament as text/JSON.
