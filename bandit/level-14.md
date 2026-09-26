# Bandit Level 14 → Level 15

## Goal

Submit the current password to a service on `localhost`, port `30000`, to retrieve the next password.

## Concept

Netcat (`nc`) establishes TCP connections using a hostname and port. Here, `localhost` refers to the Bandit server because the command runs inside its SSH session.

## Commands Used

```bash
nc localhost 30000
```

## What I Did

I connected to the service using Netcat and submitted the current password. The service returned the password for the next level.

## What I Learned

This challenge demonstrated the difference between SSH, which provides remote access, and Netcat, which allows direct communication with network services.

It also reinforced how hostnames and ports identify the destination of a network connection.

