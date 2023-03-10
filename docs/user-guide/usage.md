# Usage

You may need the utilities from the [pboted-tools](https://github.com/polistern/pboted-tools/) repository to work with **pboted**.   
In the near future, this functionality will be implemented as a [CLI utility](https://github.com/polistern/pbotectl).

You can only continue to use your `Email identities` from **i2p.i2p-bote** if:

- address is created using the one of the follow algorithm (others are not supported yet):
    - `ECDH-256/ECDSA-256/AES-256/SHA-256`
    - `ECDH-521/ECDSA-521/AES-256/SHA-512` 
- identities file is not encrypted (encrypted files are not supported yet)

## Create Email identity

!!! note "Note"

    After making changes to `identities.txt` file the application must be restarted

**Email identity** in I2P/Bote network is the equivalent of a **mailbox** in regular email.
After its creation, you will be able to send and receive emails.

Before you get started, you need to create an email identity yourself.
See [create_identity](https://github.com/polistern/pboted-tools/tree/main/create_identity)

## Sending email

!!! note "Note"

    After successful sending, the email file will be modified and moved to the `sent` directory

- Any valid MIME file with valid `FROM` and `TO` headers in `outbox` directory will be sent
- Follow headers will be added to the file after initial processing:
    - `X-HashCash`
    - `X-I2PBote-Signature`
- Will be created `<Message-ID>.meta` service information file in `outbox` directory
- If mail sent successfully files `<Message-ID>.mail` and `<Message-ID>.meta` will be moved to `sent` directory

### SMTP

!!! note "Note"

    When sending via SMTP, a MIME file will also be created in the `outbox` directory

See [SMTP](../tutorials/SMTP.md)

### Via `outbox` directory 

- Prepare plain test message
- Format it with [message_formatter](https://github.com/polistern/pboted-tools/tree/main/message_formatter)
- Put result file to `outbox` directory in pboted working directory
- pboted will automatically check `outbox` and send email

## Receiving email

After starting with a generated identity the application will begin its normal job of searching for mail.  

### Via `inbox` directory 

If mail for identity are found, they will be placed in the `inbox` directory as a plain text file.

### POP3

See [POP3](../tutorials/POP3.md)

## Third party applications

You can use any third party application that supports **SMTP** and **POP3** protocols.

Be aware that there is significant latency on the **I2P** network.  
It also adds a delay from the **I2P/Bote** network itself.

Otherwise, any application should treat **pboted** as normal email server.
