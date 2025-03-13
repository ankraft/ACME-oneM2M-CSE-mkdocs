# Hashing Credentials

Passwords, authorization tokens, and other credentials should never be stored in plain text. Instead, they should be hashed using a secure hashing algorithm. This ensures that even if the data is compromised, the original credentials cannot be recovered.

The tool that is provided in the directory [tools/hashcreds](https://github.com/ankraft/ACME-oneM2M-CSE/blob/master/tools/hashcreds){target=_new} can be used to create hashed credentials for the ACME CSE, especially for the password and token files for [HTTP](../../setup/Configuration-http.md#security) and [WebSocket](../../setup/Configuration-ws.md#security) authentication in the *certs* directory. See [Certificates](../../setup/Certificates.md) for more information.

## Running

The tool can be used by running the command from the `tools/hashcreds` directory. 

The tool requires two arguments: the password or token to be hashed, and a secret. This secret or salt value should be a secret value that is unique to the installation. It must be the same value that is configured for the CSE's [secret](../../setup/Configuration-cse.md#general-security).
The salt value is used to make the hash unique and to prevent dictionary attacks.	

=== "Running the Hashing Tool"
	```bash
	python3 hashcreds.py myPassword secret
	```

=== "Output"
	3fadece711de4314f183fc475b0b6831f4fea0541cf91e7076a1f932073cf0e4


This will create a hashed version of the password `myPassword` and print it to the console. This hash can then be copied to the password or token file in the *certs* directory.

