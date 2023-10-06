[home](../README.md)

# LogicApp workflows

For an easy integration of Azure B2C to send a custom reset password email we use Azure Logic App

## Send pwd reset email with magic link

- triggered from a custom B2C sign-in policy when user click on forgot password link
- GET id-token from custom policy
- generate **magic link**
- send email to the user

# Setup

