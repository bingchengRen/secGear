# Getting started with the sign_tool

The sign_tool.sh helps to sign the enclave.

## The sign_tool.sh

The sign_tool.sh uses the 'sgx_sign' tool in SGX SDK for signing the SGX enclave and the 'sign_too.py' for signing the trustzone enclave.

The tool supports the following two modes:


- single-step method, it is only for the dubug mode.  

    For example:    

    `$ ./signtool.sh –d sign –x 2 –i test.enclave -m manifest.txt –e device_pubkey.pem –o signed.enclave `


- two-step method, it is used when the signature needs to be obtained from the signing organization or the private key is stored on another secure platform.  

    For example:  
    (1) generate the digest value.  
    `$ ./signtool.sh –d digest –x 2 –i input -m manifest.txt –e device_pubkey.pem –o digest.data `

    For trustzone, temporary files KeyInfo.enc, rawData.enc, and rawDataHash.bin are generated in the current directory. And for SGX, a temporary file signdata is generated in the current directory. The temporary file is required when generating the signed enclave in step 3 and is deleted after the signed enclave is generated.  

    (2) send the digest.data to the signing organization or platform and get the signature.  

    (3) use the signature to generate the signed enclave.  
    `$ ./signtool.sh –d sign –x 2 –i input -m manifest.txt –p pub.pem –e device_pubkey.pem –s signature –o signed.enclave `

## sign_tool.sh parameter

```
    -d <parameter>: sign tool command, sign/digest.
                    The sign command is used to generate a signed enclave.
                    The digest command is used to generate a digest value.
    -i <file>:      enclave to be signed.
    -x <parameter>: enclave type, 1: SGX, 2:trustzone.
    -m <file>:      manifest file, required by trustzone.
    -a <parameter>: API_LEVEL, indicates trustzone GP API version, defalut is 1.
    -f <parameter>: OTRP_FLAG, indicates whether the OTRP standard protocol is supported, default is 0.
    -t <parameter>: trustzone TA_TYPE, default is 1.
    -c <file>:      config file.
    -k <file>:      private key required for single-step method required when trustzone TA_TYPE is 2 or SGX.
    -p <file>:      signing server public key certificate, required for two-step method.
    -s <file>:      the signed digest value required for two-step method, this parameter is empty to indicate single-step method.
    -e <file>:      the device's public key certificate, used to protect the AES key of the encrypted rawdata, required by trustzone.
    -o <file>:      output parameters, the sign command outputs sigend enclave, the digest command outpus digest value.
    -h:             printf help message.
```
**Note**: 
Using the `./sign_tool.sh -h` to get help information.
For trustzone, it will randomly generate a AES key and temporarily stored in the file in plaintext, please ensure security.