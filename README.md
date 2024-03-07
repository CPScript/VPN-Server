# HOW TO MAKE YOUR PHONE HOST A `OpenVPN` SERVER USING TERMUX

> I dont know why i'm doing this... BUT

## Step 1: Install Termux
First, you need to `install Termux` from the Google Play Store or `F-Droid`. (I like to install it using `F-Droid`)

## Step 2: Install OpenVPN
`OpenVPN` is a popular VPN protocol that is highly secure and easy to set up. You can install it on Termux by running the following commands:
```
pkg update
pkg install openvpn
```
Very simple :P

## Step 3: Generate Certificates and Keys
OpenVPN requires a set of `certificates and keys` to function. 

You can generate these using the `easy-rsa` package. Install it and then use it to generate your keys:
```
pkg install easy-rsa
easyrsa init-pki
easyrsa build-ca
easyrsa gen-dh
easyrsa build-server-full server nopass
easyrsa build-client-full client nopass
```
This will create a set of certificates and keys in the `pki` directory under your home directory.


## Step 4: Configure OpenVPN
You need to create a configuration file for OpenVPN. Create a new file named `server.conf` and add the following content:

```
port 1194
proto udp
dev tun
ca /path/to/your/pki/ca.crt  > Replace path!
cert /path/to/your/pki/server.crt  > Replace path!
key /path/to/your/pki/server.key  > Replace path!
dh /path/to/your/pki/dh.pem  > Replace path!
server 10.8.0.0 255.255.255.0
ifconfig-pool-persist ipp.txt
push "redirect-gateway def1 bypass-dhcp"
push "dhcp-option DNS 208.67.222.222"
push "dhcp-option DNS 208.67.220.220"
keepalive 10 120
cipher AES-256-CBC
user nobody
group nogroup
persist-key
persist-tun
status openvpn-status.log
verb 3
```
Replace `/path/to/your/pki/` with the actual path to your `pki` directory.

## Step 5: Start OpenVPN Server
Now you can start the OpenVPN server by running:
```
openvpn --config /path/to/your/server.conf
```

## Step 7: Connect to the VPN
On a differant device, use an OpenVPN client app to connect to the VPN using the `client.ovpn` file. You will need to enter the username and password you set when generating the client certificate.
