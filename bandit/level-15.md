# Bandit Level 15 → Level 16

## Goal

Submit the current password to a service on `localhost`, port `30001`, using an encrypted SSL/TLS connection.

## Concept

TLS encrypts network communication. The `openssl s_client` command establishes a TLS connection to a server.

## Commands Used

```bash
openssl s_client -connect localhost:30001 -quiet -nocommands
```

## What I Did

I established a TLS connection and submitted the current password to retrieve the next one.

The `-connect` option specifies the hostname and port, `-quiet` reduces unnecessary output, and `-nocommands` disables OpenSSL's interactive commands.

## What I Learned

This challenge demonstrated how OpenSSL can communicate with TLS-enabled services, building on the previous Netcat exercise.

It also reinforced that encryption and server identity verification are separate: an encrypted connection doesn't automatically mean the server is trusted.

