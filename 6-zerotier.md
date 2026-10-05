# ZeroTier Installation

This section walks through installing ZeroTier One for connecting your node to a virtual network.
>🛈 Note: Complete these steps after you've create a ZeroTier account. You get up to 10 free licenses.

## Install ZeroTier One

Run the ZeroTier install script:
```
curl -s https://install.zerotier.com | sudo bash
```

Check that the background service (zerotier-one) is active and running:
```
sudo systemctl status zerotier-one --no-pager
```
>🛈 Expected Output:
><br>
><br>● zerotier-one.service - ZeroTier One
     <br>Loaded: loaded (/lib/systemd/system/zerotier-one.service; enabled; vendor preset: enabled)
     <br>Active: active (running) since ...

Confirm that your local daemon is responding and identify your **10-digit Node ID**:
```
sudo zerotier-cli info
```
>🛈 Expected Output:
><br>
><br>200 info 1a2b3c4d5e 1.12.2 ONLINE

Join your specified private network by replacing <YOUR_16_DIGIT_NETWORK_ID> with your actual network ID:
```
sudo zerotier-cli join <YOUR_16_DIGIT_NETWORK_ID>
```
>🛈 Expected Output:
><br>
><br> 200 join OK

## Authorize the Device

Joining the network notifies the network controller, but traffic will not flow until authorized:

- Log into your ZeroTier Central Console (or ask your network administrator).

- Open your network and scroll down to the Members section.

- Locate the row matching the **10-digit Node ID**.

- Check the **Auth?** checkbox to approve the device.

- Once authorized, ZeroTier will assign a virtual managed IP address to your device.

## Verify Netowrk Connection

To verify that your node has joined and received an IP address, run:
```
sudo zerotier-cli listnetworks
```
>🛈 Note:
>- Status should read OK (not ACCESS_DENIED or REQUESTING_CONFIGURATION).
>- A virtual interface (usually zt0 or zt<interface-id>) should now show an assigned managed IP.
