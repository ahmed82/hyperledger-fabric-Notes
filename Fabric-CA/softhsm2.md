# Install softhsm2 For Fabric development environment


```shell
cd ~
wget  https://dist.opendnssec.org/source/softhsm-2.5.0.tar.gz
tar -xzf softhsm-2.5.0.tar.gz
cd softhsm-2.5.0/
./configure
make
make install 
```


```shell
apt-get install build-essential
apt-get install openssl automake autoconf libtool libssl-dev
git clone https://github.com/opendnssec/SoftHSMv2.git
cd SoftHSMv2/
sh autogen.sh
./configure
# Compile the source code using the following command:
make
# Install the library using the follow command:
sudo make install
```
### Location of the configuration file.
The default location of the config file is /etc/softhsm2.conf. This location can be change by setting the environment variable.
```shell
export SOFTHSM2_CONF=/home/user/config.file
```
or you can `export SOFTHSM2_CONF=/etc/softhsm2.conf` # Optionaly

## Configure SoftHSM
A PKCS #11 cryptographic token implementation is required to run the unit tests. The PKCS #11 API is used by the bccsp component of Fabric to interact with hardware security modules (HSMs) that store cryptographic information and perform cryptographic computations. For test environments, SoftHSM can be used to satisfy this requirement.

SoftHSM generally requires additional configuration before it can be used.

## Initialize the token required by the unit tests:
```shell
softhsm2-util --init-token --slot 0 --label ForFabric --so-pin 1234 --pin 98765432
```

If tests are unable to locate the libsofthsm2.so library in your environment, specify the library path, the PIN, and the label of your token in the appropriate environment variables.
### SoftHSM on ubuntu environment variables
```shell
export PKCS11_LIB="/usr/local/lib/softhsm/libsofthsm2.so"
export PKCS11_PIN=98765432
export PKCS11_LABEL="ForFabric"
```

Once a token has been initialized, more slots will be added automatically with a new uninitialized token.

The tests don’t always clean up after themselves and, over time, this causes the PKCS #11 tests to take a long time to run. The easiest way to recover from this is to delete and recreate the token.
```shell
# Delete the exist token
softhsm2-util --delete-token --token ForFabric
# Initialize the token again
softhsm2-util --init-token --slot 0 --label ForFabric --so-pin 1234 --pin 98765432
```

## Debugging with pkcs11-spy
The **OpenSC Project** provides a shared library called `pkcs11-spy` that logs all interactions between an application and a PKCS #11 module.
Once the library has been installed, configure Fabric to use pkcs11-spy as the PKCS #11 library and set the PKCS11SPY environment variable to the real library.
```shell
export PKCS11SPY="/usr/lib/softhsm/libsofthsm2.so"
export PKCS11_LIB="/usr/lib/x86_64-linux-gnu/pkcs11/pkcs11-spy.so"
```
![image](https://user-images.githubusercontent.com/9446035/215963467-118568b2-f4be-48fc-95f2-e71ff7eb3f81.png)

*Ref: 
1. https://wiki.opendnssec.org/display/SoftHSMDOCS/SoftHSM+Documentation+v2#SoftHSMDocumentationv2-Download
2. https://hyperledger-fabric.readthedocs.io/en/latest/dev-setup/devenv.html

