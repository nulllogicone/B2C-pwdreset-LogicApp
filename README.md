# B2C custom password reset with LogicApp

We use Azure Active Directory B2C to authenticate users of our mobile- and web- application. The default build in experience does not reflect our corporate identity and is not very convenient. 

## Build in flow

User clicks on 'forgot password', enters his email address to send a confirmation code.  

The email is very minimalistic, has the subject "Microsoft on behalf of YourTenantName" and is sent from a Microsoft account.
There is a 6 digit code to be copied back to the B2C page to verify that you received the email and then you can provide a new password.

## Desired custom flow

User clicks on 'forgot password', enters his email address and ...  

A fully customized email from an own email address with a 'magic link' to change the password is sent to the user.
The link verifies that the user has access to his inbox and expire after a short time (60 min)

A nice example is Twilio SendGrid and the login screens look like they use Azure B2C, I'll take the screenshots from them

## Solution

### B2C tenant with 3 Custom policies

To try this example with your own B2C tenant you must configure it to use custom policies as described [here](https://learn.microsoft.com/en-us/azure/active-directory-b2c/tutorial-create-user-flows?pivots=b2c-custom-policy)

The easiest way to experiment with the xml policy files is to edit them locally and upload them to Azure with the vscode extension that replaces all tenant specific values with your own (https://github.com/azure-ad-b2c/vscode-extension/blob/master/src/help/policy-upload.md)

In addition you need to add one more App registration for the Logic App with a redirect URL of https://jwt.ms that allows to issue Access tokens (used for implicit flows). You'll need the ClientId later in the LogicApp to get the token.

#### SignUpOrSignIn

This is derived from the default starter pack policy but overwrites the 'forgot password link' with a subjourney that shows 
a textbox to the user to enter his email address and makes a REST API call to a Logic App to send an email

#### GeneratePasswordToken

This policy has no user interface and gets called from an action in the LogicApp. It generates a token with the 
provided email address, an expiration time and signature.

#### PasswordReset

The policy validates the id_token signature and expiration and then shows a form with the read only email
and two text boxes to provide a new password and confirm the new password.

### 1 Logic App workflow

For this scenario we only need one workflow, so consumption plan is fine.
The flow is pretty straight forward: 
- the first action immediately returns a 200 OK response that B2C doesn't need to wait.
- three actions initialize variables for baseUrl, id_token and the full "magic link" 
- then comes a GET request to the custom B2C **GeneratePasswordToken** policy endpoint 
- that returns a 302 redirect 
- so the following action is a Condition that checks the status code and is configured to also 'Run After' = 'Has failed' 
- we can grab the token from the Location header as substring after #id_token url fragment.
- and concatenate the baseUrl with the id_token to have the full **magic link** 
- that can be send with any email provider like SendGrid (not shown here, for this example I just open the run history 
  to copy the link from the output of the last action to paste it in a browser)

## Nice to have next

- B2C should not indicate whether an account exists and always show message 'if account exists, an email was sent'
- check existence of account in LogicApp before creating link (make sure this workflow does not get fludded by brute force requests)
- confirm reset password to the user (another LogicApp workflow invoked by PasswordReset policy)

*** UPDATE ****
There is now a preview feature in B2C for custom email verification with SendGrid so this manual customization might become obsolete but helps to understand how it works.
