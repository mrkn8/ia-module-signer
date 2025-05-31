# Getting started with module-signer

## Prerequisites

* Java 11 installed and on your path.
* One of the following:
  * A Java Keystore (in jks or pfx format) containing either:
    * A self-generated and self-signed code signing certificate.
    * A code signing certificate, obtained from and signed by a CA, and the certificate chain that goes with it.
  * A hardware token (YubiKey FIPS, SafeNet eToken, etc.)
    * A code signing certificate, obtained from and signed by a CA, imported to the hardware token
    * The aforementioned certificate chain saved in .p7b format

[Keystore Explorer](http://keystore-explorer.sourceforge.net/downloads.php) is an easy to use tool for creating and managing keystores and certificates.

## Invocation

Invocation from the command-line:
```
java -jar module-signer.jar \ 
	-keystore=<path-to-my-keystore>/keystore.jks \
	-keystore-pwd=<password-or-pin> \
	-alias=server \
	-alias-pwd=<password> \
	-chain=<pathToMyp7b>/cert.p7b \
	-module-in=<path-to-my-module>/my-unsigned-module.modl \
	-module-out=<path-to-my-module>/my-signed-module.modl \
	-pkcs11-cfg=<path-to-cfg>/eToken.cfg \
	-algo=SHA512withRSA
```

## Parameters Explained

### keystore
The path to the keystore containing your code signing certificate. Can be either JKS or PFX format.
*Exclude this parameter if using a hardware token.*

### keystore-pwd
The password to access the keystore, or PIN if using hardware token.

### alias
The alias under which your code signing certificate is stored.<br>
*If using a YubiKey with your Code Signing Certificate in slot 9a, this should be set to "Certificate for PIV Authentication"*

### alias-pwd
The password to access the alias. *Exclude this parameter if using a hardware token*

### chain
The path to the certificate chain (in p7b format). This file will is generally returned along with your signed certificate after submitting a CSR to a CA.

### module-in
The path to the unsigned module.

### module-out
The path the signed module will be written to.

### pkcs11-cfg
The path to the configuration file for hardware token support (YubiKey, SafeNet, etc.)

### algo
The algorithm used to sign your module with, only necessary if the private key algorithm cannot be derived automatically.<br>
Supported values: `SHAH256withRSA`, `SHA256withECDSA`, `SHA384withECDSA`, `SHA512withECDSA`
