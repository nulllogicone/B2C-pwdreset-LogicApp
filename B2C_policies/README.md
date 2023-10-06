[home](../README.md)

# B2C policies

This folder holds the source of all custom policies.  

The easiest way to experiment with those files is to edit them locally and upload them to Azure with the vscode extension that replaces the tenant name with your own (https://github.com/azure-ad-b2c/vscode-extension/blob/master/src/help/policy-upload.md)

Before you upload the TrustFrameworkExtensions.xml file you would need to replace the IdentityExperienceFrameworkId and ProxyIdentityExperienceFramework with your own values that you received from [initial setup](https://learn.microsoft.com/en-us/azure/active-directory-b2c/tutorial-create-user-flows?pivots=b2c-custom-policy) 

## GenerateResetToken.xml

Returns an id-token to be used in a **magic reset link** 

## ResetPassword.xml

# Add Application Insights
https://learn.microsoft.com/en-us/azure/active-directory-b2c/troubleshoot-with-application-insights?WT.mc_id=AZ-MVP-5003445&pivots=b2c-custom-policy

