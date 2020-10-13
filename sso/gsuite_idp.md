# Notes for setup Federated Single Sign-On to AWS Using Google Apps

[Federated SSO to AWS Using Google Apps](https://aws.amazon.com/blogs/security/how-to-set-up-federated-single-sign-on-to-aws-using-google-apps/)

1. Log in to your Google Admin console with your super administrator credentials.

2. Security -> SSO with Google as SAML IdP

- Get IdP ID at the end of the SSO URL
  - https://accounts.google.com/o/saml2/idp?idpid=C0236e2au
    -> IdP ID: C0236e2au
- Download IdP Metadata

3. Create IdP in AWS Account

> IAM Console -> Identity Providers -> Create Provider -> SAML

- Name: KarrosGsuiteSSO
- Upload IdP metadata from above
- Create
- Get Provider ARN: arn:aws:iam::3421821342342:saml-provider/KarrosGsuiteSSO

4. Create IAM Role for using

Use CFN Template

5. Create Schema with Google Directory API

```
POST https://developers.google.com/admin-sdk/directory/v1/reference/schemas/insert#try-it

customerId: {IdP ID}

BODY:
{
	"displayName": "AWSSSO",
  "schemaName": "AWSSSO",
  "fields":
  [
    {
      "fieldType": "STRING",
      "fieldName": "role",
      "displayName": "AWSSSO",
      "multiValued": true,
      "readAccessType": "ADMINS_AND_SELF"
    }
  ]
}
```

6. Add Role to Gsuite User Schema

Many AWS Accounts and Permissions can be controlled by adding user schema Roles as below:

```
POST https://developers.google.com/admin-sdk/directory/v1/reference/users/patch#try-it

userKey: {userEmail}

BODY:
{
  "customSchemas": {
    "SSO": {
      "role": [
        {
          "customType": "ATH-Developers",
          "value": "{AWS_Main_IAM_Role_ARN},{AWS_Main_IAM_IDP_ARN}"
        },
        {
          "customType": "ATH-Developers",
          "value": "{AWS_Athena_IAM_Role_ARN},{AWS_Athena_IAM_IDP_ARN}"
        }
      ]
    }
  }
}
```

AWS_IAM_Role_ARN: arn:aws:iam::69692341346624:role/athena-iam-AthGsuiteDeveloperRole-1CUN0RU8UPHK9
AWS_IAM_IDP_ARN: arn:aws:iam::69695334234624:saml-provider/KarrosGsuiteSSO

7. Set up the SAML app in Google Apps

> Google Admin console -> Apps -> SAML apps -> New App -> Amazon Web Services

- Set up Google as IdP
- Next
- Provide basic information for AWS
  - Application Name
  - Description
  - Logo
- Provide service provider details
  - ACS URL (Assertion Consumer Service URL) and Entity ID
  - Name ID: Basic Information > Primary Email
- SAML Attribute mapping
  - https://aws.amazon.com/SAML/Attributes/Role
    AWSSSO > Role
  - https://aws.amazon.com/SAML/Attributes/RoleSessionName
    Basic Information > Primary email

8. Enable App for Everyone or on some organizations

9. Using

Open new tab in browser -> Apps -> Roll down -> Amazon Web Services -> Choose the correct role
