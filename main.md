## 1. Create Resource Group

#### Command
``` 
az group create --name VnetQS-rg --location eastus
```
#### Output

```
{
  "id": "/subscriptions/b8b7261c-***TRUNCATED***d3172ac0d13d/resourceGroups/VnetQS-rg",
  "location": "eastus",
  "managedBy": null,
  "name": "VnetQS-rg",
  "properties": {
    "provisioningState": "Succeeded"
  },
  "tags": null,
  "type": "Microsoft.Resources/resourceGroups"
}
```

## 2. Create Virtual Network

#### Command

```
az network vnet create --name myVNet --resource-group VnetQS-rg --subnet-name default
```

#### Output

```
{
  "newVNet": {
    "addressSpace": {
      "addressPrefixes": [
        "10.0.0.0/16"
      ]
    },
    "enableDdosProtection": false,
    "etag": "W/\"018aa59a-***TRUNCATED***-ea008c173cba\"",
    "id": "/subscriptions/b8b7261c-***TRUNCATED***d3172ac0d13d/resourceGroups/VnetQS-rg/providers/Microsoft.Network/virtualNetworks/myVNet",
    "location": "eastus",
    "name": "myVNet",
    "provisioningState": "Succeeded",
    "resourceGroup": "VnetQS-rg",
    "resourceGuid": "2a0c1ea1-***TRUNCATED***-c5c13f354d89",
    "subnets": [
      {
        "addressPrefix": "10.0.0.0/24",
        "delegations": [],
        "etag": "W/\"018aa59a-***TRUNCATED***-ea008c173cba\"",
        "id": "/subscriptions/b8b7261c-***TRUNCATED***d3172ac0d13d/resourceGroups/VnetQS-rg/providers/Microsoft.Network/virtualNetworks/myVNet/subnets/default",
        "name": "default",
        "privateEndpointNetworkPolicies": "Disabled",
        "privateLinkServiceNetworkPolicies": "Enabled",
        "provisioningState": "Succeeded",
        "resourceGroup": "VnetQS-rg",
        "type": "Microsoft.Network/virtualNetworks/subnets"
      }
    ],
    "type": "Microsoft.Network/virtualNetworks",
    "virtualNetworkPeerings": []
  }
}
```

## 3. Create VM

#### Command 

```
az vm create --resource-group VnetQS-rg --name myVM1 --image UbuntuLTS --generate-ssh-keys --public-ip-address myPublicIP-myVM1 \
```

#### Output

```
{
  "fqdns": "",
  "id": "/subscriptions/b8b7261c-***TRUNCATED***d3172ac0d13d/resourceGroups/VnetQS-rg/providers/Microsoft.Compute/virtualMachines/myVM1",
  "location": "eastus",
  "macAddress": "00-22-***TRUNCATED***-99",
  "powerState": "VM running",
  "privateIpAddress": "10.0.0.4",
  "publicIpAddress": "13.XX.XX.196",
  "resourceGroup": "VnetQS-rg",
  "zones": ""
}
```

## 4. Get Name of the IP configuration of the network interface

#### Command 

```
az network nic ip-config list --nic-name myVM1VMNic --resource-group VnetQS-rg --out table
```

#### Output

```
Name           Primary    PrivateIpAddress    PrivateIpAddressVersion    PrivateIpAllocationMethod    ProvisioningState    ResourceGroup
-------------  ---------  ------------------  -------------------------  ---------------------------  -------------------  ---------------
ipconfigmyVM1  True       10.0.0.4            IPv4
```

## 5. Get Network interface name attached to VM

#### Command

```
az vm nic list --vm-name myVM1 --resource-group VnetQS-rg
```

#### Output

```
{
    "deleteOption": null,
    "id": "/subscriptions/b8b7261c-***TRUNCATED***d3172ac0d13d/resourceGroups/VnetQS-rg/providers/Microsoft.Network/networkInterfaces/myVM1VMNic",
    "primary": null,
    "resourceGroup": "VnetQS-rg"
  }
```

## 6. Remove Public IP from VM

#### Command

```
az network nic ip-config update --name ipconfigmyVM1 --resource-group VnetQS-rg --nic-name myVM1VMNic --public-ip-address ''
```

#### Output

```
{
  "applicationGatewayBackendAddressPools": null,
  "applicationSecurityGroups": null,
  "etag": "W/\"13e294cc-***TRUNCATED***-7741db41f323\"",
  "gatewayLoadBalancer": null,
  "id": "/subscriptions/b8b7261c-***TRUNCATED***d3172ac0d13d/resourceGroups/VnetQS-rg/providers/Microsoft.Network/networkInterfaces/myVM1VMNic/ipConfigurations/ipconfigmyVM1",
  "loadBalancerBackendAddressPools": null,
  "loadBalancerInboundNatRules": null,
  "name": "ipconfigmyVM1",
  "primary": true,
  "privateIpAddress": "10.0.0.4",
  "privateIpAddressVersion": "IPv4",
  "privateIpAllocationMethod": "Dynamic",
  "privateLinkConnectionProperties": null,
  "provisioningState": "Succeeded",
  "publicIpAddress": null,
  "resourceGroup": "VnetQS-rg",
  "subnet": {
    "addressPrefix": null,
    "addressPrefixes": null,
    "applicationGatewayIpConfigurations": null,
    "delegations": null,
    "etag": null,
    "id": "/subscriptions/b8b7261c-***TRUNCATED***d3172ac0d13d/resourceGroups/VnetQS-rg/providers/Microsoft.Network/virtualNetworks/myVNet/subnets/default",
    "ipAllocations": null,
    "ipConfigurationProfiles": null,
    "ipConfigurations": null,
    "name": null,
    "natGateway": null,
    "networkSecurityGroup": null,
    "privateEndpointNetworkPolicies": null,
    "privateEndpoints": null,
    "privateLinkServiceNetworkPolicies": null,
    "provisioningState": null,
    "purpose": null,
    "resourceGroup": "VnetQS-rg",
    "resourceNavigationLinks": null,
    "routeTable": null,
    "serviceAssociationLinks": null,
    "serviceEndpointPolicies": null,
    "serviceEndpoints": null,
    "type": null
  },
  "type": "Microsoft.Network/networkInterfaces/ipConfigurations",
  "virtualNetworkTaps": null
}
```