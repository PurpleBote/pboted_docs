# POP3

_Tested with [Mozilla Thunderbird](https://www.thunderbird.net/en-US/)_

To be able to receive email through POP3 you need to:

- Fill `[pop3]` section in [configuration file](../user-guide/configuration.md#pop3):

```ini
[pop3]
enabled = true
address = 127.0.0.1
port = 9110
```

- Restart the **pboted** to apply the settings
- After loading, you be able to connect to the specified POP3 port manually or with your mail client.
