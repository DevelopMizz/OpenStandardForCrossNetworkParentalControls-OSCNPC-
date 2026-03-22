# OpenStandardForCrossNetworkParentalControls-OSCNPC-
This repository contains and open standard that allows supporting devices to share parental control policies with any network they join that implaments the standard.

## Overview
| Standard version | Publication date |
|------------------|------------------|
| Alpha 0.0.0.1    | 2026-March-22nd  |

The standard defines 2 HTTP endpoints that should be exposed by the gateway on TCP port 4080. 
Those endpoints are:
- `HTTP GET` on `/ParentalControls/OSCNPC`. This endpoint must return 200 OK if the standard is supported.
- `HTTP POST` on `/ParentalControls/OSCNPC` with payload
```JSON
{
  "MAC":<Device interface MAC address>,
  "IPv4":<Device interface IPv4 address>, // valid combinations are (IPv4 & IPv6) or (IPv4) or (IPv6)
  "IPv6":<Device interface IPv6 address>,
  "Policies":{
    DNSChildSafeBlocklist: <Boolean>, //enable or disable
    ClientIsolation: <Boolean>, //enable or disable
    DisallowProxies  <Boolean>, //enable or disable (DisallowVPNs will be forced to TRUE for this policy)
    DisallowVPNs  <Boolean>, //enable or disable
    AllwaysBlock:[<urls>],
    AllowIfPermittedByNetwork:[<urls>],
    LimitInternetTime:{
      AllowedBetween {
        Start:<DateTime>,
        End:<DateTime>
      },
      LimitDuration: <int32> // Number of minutes per day
    }
  }
}
```


