# file-encryption
File encryption using a combination of AES-256 together with public-key encryption.
Public-key encryption is not ideal for encrypting large amounts of data.
Instead, we encrypt files using AES-256 encryption with a unique randomly generated passphrase for each file.
Then we encrypt the AES-256 keys using public-key encryption, and store this inside each file.
Decryption is a matter of reversing this process: decrypt the AES-256 key using the private key, and then decrypt the file.

## Scripts

### genkeypair
This script generates a `privkey.pem` and `pubkey.pem` file.
The `pubkey.pem` file can be distributed widely and is used to encrypt.
The `privkey.pem` file should be carefully guarded; it is used to decrypt.

### encrfile
This script encrypts one or more files using the `pubkey.pem` file located in the current working directory.
Each encrypted file is saved with a `.enc` extension.

### decrfile
This script decrypts a single file and dumps the result to `stdout`.
The script searches all `privkey*.pem` files in the current working directory until it finds one that successfully decrypts the AES-256 key.
Then it decrypts the file using this AES-256 key.
Because this script supports multiple private keys, you can use this as a rudimentary key rotation mechanism.
You can generate new key pairs for new encryption, but rename and continue to use older private key files for decryption.
