## GRPC client command line tutorial and examples

We recently opensourced a python based GRPC client designed to interact with IOS-XR 6.x:

[https://xrdocs.github.io/programmability/tutorials/2016-11-03-grpc-in-python-for-ios-xr/](https://xrdocs.github.io/programmability/tutorials/2016-11-03-grpc-in-python-for-ios-xr/)

[https://github.com/cisco-grpc-connection-libs/ios-xr-grpc-python](https://github.com/cisco-grpc-connection-libs/ios-xr-grpc-python)

The client now includes CLI options and a number of additional sample json snips to include with your RPCs.  This tutorial walks through the use of the GRPC Client CLI and examples.

What you'll need:

1. Management station/laptop: 
```
sudo apt-get update
sudo apt-get -y install python-dev python-pip git
sudo pip install grpcio
```

2. One or more reachable IOS-XR routers running 6.x code.  Note: the client has been tested mostly on 6.1.1
3. Configure GRPC server on your router(s)

```
grpc

 port 57400
```
 
The CLI doesn't currently support TLS encryption, so we've left that option disabled.

Next, clone the GRPC python client to your management station:

```
git clone https://github.com/cisco-grpc-connection-libs/ios-xr-grpc-python.git
cd ios-xr-grpc-python/examples/
```
In the examples directory you'll find several python sample scripts the cli.py tool, and json/ directory containing example RPC filters.


# 1. Perform an unfiltered get-config operation:

This command should return the router's entire config in json

```
python cli.py -i 192.168.122.214 -p 57400 -u cisco -pw cisco -r get-config

```

Output would look something like a much longer version of this:

```
{
 "data": {
  "Cisco-IOS-XR-shellutil-cfg:host-names": {
   "host-name": "router1"
  },
  "Cisco-IOS-XR-man-ems-cfg:grpc": {
   "port": 57400,
   "enable": [
    null
   ]
  }
  <snip>
```

# 2. Perform an filtered get-config operation:

In this example we'll just ask for interface config:

```
python cli.py -i 192.168.122.214 -p 57400 -u cisco -pw cisco -r get-config --file json/get-config-interfaces.json

```

Partial output:

```
{
 "Cisco-IOS-XR-ifmgr-cfg:interface-configurations": {
  "interface-configuration": [
   {
    "active": "pre",
    "interface-name": "MgmtEth0/0/CPU0/0",
    "Cisco-IOS-XR-ipv4-io-cfg:ipv4-network": {
     "addresses": {
      "dhcp": [
       null
      ]
     }
    }
   },
   {
    "active": "act",
    "interface-name": "Loopback0",
    "interface-virtual": [
     null
    ],
    "Cisco-IOS-XR-ipv4-io-cfg:ipv4-network": {
     "addresses": {
      "primary": {
       "address": "2.2.1.2",
       "netmask": "255.255.255.255"
      }
     }
    },
    "Cisco-IOS-XR-ipv6-ma-cfg:ipv6-network": {
     "addresses": {
      "regular-addresses": {
       "regular-address": [
        {
         "address": "2:2:1::2",
         "prefix-length": 128,
         "zone": 0
        }
       ]
      }
     }
    }
   },
   {
    "active": "act",
    "interface-name": "GigabitEthernet0/0/0/0",
    "description": "to router2",
    "Cisco-IOS-XR-ipv4-io-cfg:ipv4-network": {
     "addresses": {
      "primary": {
       "address": "2.2.2.35",
       "netmask": "255.255.255.254"
      }
     }
    },
    "Cisco-IOS-XR-ipv6-ma-cfg:ipv6-network": {
     "addresses": {
      "regular-addresses": {
       "regular-address": [
        {
         "address": "2:2:2::35",
         "prefix-length": 127,
         "zone": 0
        }
       ]
      }
     }
    }
   }
   ```
Note the json/ directory has numerous example get-config (and other RPC) filters

# 3. Perform an filtered get-oper request:

Due to the fact that an unfiltered get-oper (get-oper *.*) might take years to complete, the CLI only allows filtered get-oper requests.  In this example we'll ask for a subset of BGP RIB data:

```
python cli.py -i 192.168.122.214 -p 57400 -u cisco -pw cisco -r get-oper --file json/get-oper-cisco-oc-bgp-rib.json
```

Partial output:

```
{
 "Cisco-IOS-XR-ipv4-bgp-oc-oper:oc-bgp": {
  "bgp-rib": {
   "afi-safi-table": {
    "ipv4-unicast": {
     "loc-rib": {
      "routes": {
       "route": [
        {
         "route": "2.2.1.0/32",
         "neighbor-address": "2.2.1.102",
         "path-id": 0,
         "prefix-name": {
          "prefix": {
           "afi": "ipv4",
           "ipv4-address": "2.2.1.0"
          },
          "prefix-length": 32
         },
         "route-attr-list": {
          "origin-type": "igp",
          "as-path": [
           null
          ],
          "as4-path": [
           null
          ],
          "next-hop": {
           "afi": "ipv4",
           "ipv4-address": "2.2.1.0"
          },
          "med": 0,
          "local-pref": 100,
          "atomic-aggr": false,
          "aggregrator-attributes": {
           "as": 0,
           "as4": 0,
           "address": "0.0.0.0"
          }
         },
         "ext-attributes-list": {
          "originator-id": "2.2.1.0",
          "cluster": [
           "2.2.1.102"
          ],
          "aigp": 0,
          "path-id": 0
         },
         "last-modified-date": {
          "time-value": "2017-01-09T21:06:07Z"
         },
         "last-update-recieved": {
          "time-value": [
           null
          ]
         },
         "valid-route": true,
         "invalid-reason": "valid-route",
         "best-path": true
        }

```



Enter text in [Markdown](http://daringfireball.net/projects/markdown/). Use the toolbar above, or click the **?** button for formatting help.
