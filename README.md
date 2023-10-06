# B2C password reset with Logic App

A Blog post for i8c

## Goal

Custom password reset email with magic link  
(instead of MS mail with code to type in for confirmation)

## Solution

### custom policy 

- get id-token with expiration (no UI) for magic link
- validate link and show readonly email with two textboxes for new password

### custom Logic App

- send an email with magic link

## Setup

One GitHub repository for source code, documentation, actions and issue management. No other tools.

One Azure AD B2C tenant configured for custom policies: 
https://learn.microsoft.com/en-us/azure/active-directory-b2c/tutorial-create-user-flows?pivots=b2c-custom-policy

todo: execute the steps to Register the IdentityExperienceFramework application and describe the experience here....


### B2C_policies

[policies](/B2C_policies/README.md)

### LogicApp workflows




## Future additions

- track history of user pwd changes