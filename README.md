# OpenStandardForCrossNetworkParentalControls-OSCNPC-
This repository contains and open standard that allows supporting devices to share parental control policies with any network they join that implaments the standard.

## Version
| Standard version | Publication date |
|------------------|------------------|
| Alpha 0.0.0.1    | 2026-March-22nd  |

## Objectivres
- Make it easy for parents to enable network based parental controls for a specific device.
- Protect network based parental controls with existing device protections instead of requiring expensive and potentally risky mass age-assurance platforms.
- Define standard parental controls that all compliant devices must support.
- Allow parental controls settings to be enforced across diffrent networks.
- Allow parents to control the duration of internet use, and for this to be enforced at both the device and network level.
- Do the above securly in a way that resists misuse.

For [For network equiptment manufactures](https://github.com/DevelopMizz/OpenStandardForCrossNetworkParentalControls-OSCNPC-/blob/main/Network_Manufactures.md)

For [Client device manufactures](https://github.com/DevelopMizz/OpenStandardForCrossNetworkParentalControls-OSCNPC-/blob/main/Mobile_Device_Manufacturers)

For [Cybersecurity specailists](https://github.com/DevelopMizz/OpenStandardForCrossNetworkParentalControls-OSCNPC-/blob/main/Security_Controls.md) 
## Comon details
The standard defines 2 HTTP endpoints that should be exposed by the gateway on TCP port 4080. 
Those endpoints are:
NOTE 4080 should be replaced with an IANA registered port before real world adoption.
- `HTTP GET` on `/ParentalControls/OSCNPC`. This endpoint must return 200 OK if the standard is supported.
- `HTTP POST` on `/ParentalControls/OSCNPC` with payload
```JSON
{
  "MAC":<Device interface MAC address>,
  "IPv4":<Device interface IPv4 address>, // valid combinations are (IPv4 & IPv6) or (IPv4) or (IPv6)
  "IPv6":<Device interface IPv6 address>,
  "Policies":{
    "DNSChildSafeBlocklist": <byte>, // 0 = disabled, 1 = light, 2 = moderate, 3 = strict
    "ClientIsolation": <Boolean>, //enable or disable
    "DisallowProxies": <Boolean>, //enable or disable (DisallowVPNs will be forced to TRUE for this policy)
    "DisallowVPNs": <Boolean>, //enable or disable
    "AllwaysBlock": [<urls>],
    "AllowIfPermittedByNetwork": [<urls>],
    "LimitInternetTime":{
      "AllowedBetween":{
        "Start": <DateTime>,
        "End": <DateTime>
      },
      "LimitDuration": <int32> // Number of minutes per day
    }
  }
}
```
HTTP Response
```JSON
{
  "NumberUsedOnce": <Byte[32]>
}
```
- `HTTP POST` on `/ParentalControls/OSCNPC/Confirm` with payload
```JSON
{
  "MAC":<Device interface MAC address>,
  "IPv4":<Device interface IPv4 address>, // valid combinations are (IPv4 & IPv6) or (IPv4) or (IPv6)
  "IPv6":<Device interface IPv6 address>,
  "Confirmation": SHA256(<PolicyObject> + <NumberUsedOnce>)
}
```
This confirms that the device that submitted the policy is routable and listening on the claimed IP Address.
## Optrions 
### MAC <String>
As data type string this represents the MAC address of the network inteterface being used to connect to the network

### IPv4, IPv6 <String>
These nodes allow you to specifiy the IP addresses of the device the policy is for. ***They MUST match the sender IP address of the device that makes the requiest.*** noncompliant connections should be dropped.
If 1 of 2 supplied addresses is compliant, the policy should be applied only to the compliant address.

### Policies {}
This takes a configuration object specifing the policy to be applied.

### DNSChildSafeBlocklist <Byte>
This accepts a byte indicating both, whether a DNS blocklist is enabled, and the how strict that blocklist is.
Options are
- `0 = disabled` No protection
- `1 = light` Some protection but priorites serice quality.
- `2 = moderate` Protect the user but try not to break sites that do not contain restricted content.
- `3 = strict` Block traffic, break safe sites if needed.

### ClientIsolation <Boolean>
Where spported the client device should note be allowed to talk to other devices on the network.  

### DisallowProxies: <Boolean>
Where supported, this setting disallows connections to known proxy servers outside the network 
Implies `{DisallowVPNs = TRUE}`

### DisallowVPNs <Boolean>
Blocks common VPN protocols.

### AllwaysBlock String[]
This policy takes an array of IPs and DNS names. The network should allways block items in the list

### AllowIfPermittedByNetwork" String[]
This pollicy states that a DNS name or IP address should be whitlested unless it is otherwise blocked.

### LimitInternetTime {}
This is a parent object for the below to policies

### AllowedBetween {}
  Settings:
-   Start
-   End
This setting takes 2 times as ISO 8601 time strings that at what time internet access should be unlocked, and at what times it should be restricted. 


### LimitDuration uint32
This takes a Byte which indicates the number of hours that the device should be allowed internet access.
