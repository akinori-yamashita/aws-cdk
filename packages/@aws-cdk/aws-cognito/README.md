## Amazon Cognito Construct Library
<!--BEGIN STABILITY BANNER-->

---

![Stability: Experimental](https://img.shields.io/badge/stability-Experimental-important.svg?style=for-the-badge)

> **This is a _developer preview_ (public beta) module. Releases might lack important features and might have
> future breaking changes.**
>
> This API is still under active development and subject to non-backward
> compatible changes or removal in any future version. Use of the API is not recommended in production
> environments. Experimental APIs are not subject to the Semantic Versioning model.

---
<!--END STABILITY BANNER-->

[Amazon Cognito](https://docs.aws.amazon.com/cognito/latest/developerguide/what-is-amazon-cognito.html) provides
authentication, authorization, and user management for your web and mobile apps. Your users can sign in directly with a
user name and password, or through a third party such as Facebook, Amazon, Google or Apple.

The two main components of Amazon Cognito are [user
pools](https://docs.aws.amazon.com/cognito/latest/developerguide/cognito-user-identity-pools.html) and [identity
pools](https://docs.aws.amazon.com/cognito/latest/developerguide/cognito-identity.html). User pools are user directories
that provide sign-up and sign-in options for your app users. Identity pools enable you to grant your users access to
other AWS services.

This module is part of the [AWS Cloud Development Kit](https://github.com/aws/aws-cdk) project.

## User Pools

User pools allow creating and managing your own directory of users that can sign up and sign in. They enable easy
integration with social identity providers such as Facebook, Google, Amazon, Microsoft Active Directory, etc. through
SAML.

Using the CDK, a new user pool can be created as part of the stack using the construct's constructor. You may specify
the `userPoolName` to give your own identifier to the user pool. If not, CloudFormation will generate a name.

```ts
new UserPool(this, 'myuserpool', {
  userPoolName: 'myawesomeapp-userpool',
});
```

### Sign Up

Users can either be signed up by the app's administrators or can sign themselves up. Once a user has signed up, their
account needs to be confirmed. Cognito provides several ways to sign users up and confirm their accounts. Learn more
about [user sign up here](https://docs.aws.amazon.com/cognito/latest/developerguide/signing-up-users-in-your-app.html).

The following code snippet configures a user pool with properties relevant to when a user signs themselves up -

```ts
new UserPool(this, 'myuserpool', {
  // ...
  selfSignUpEnabled: true,
  selfSignUp: {
    verificationEmailEnabled: true,
    verificationEmailSubject: 'Verify your email for our awesome app!',
    verificationEmailBody: 'Hello {username}, Thanks for signing up to our awesome app! Your verification code is {####}',
    verificationEmailStyle: VerificationEmailStyle.CODE,

    verificationSmsEnabled: true,
    verificationSmsMessage: 'Hello {username}, Thanks for signing up to our awesome app! Your verification code is {####}',
  }
});
```

By default, self sign up is disabled. If enabled, email verification is turned on by default and SMS verification is
turned off. Learn more about [email and SMS verification
messages](https://docs.aws.amazon.com/cognito/latest/developerguide/cognito-user-pool-settings-message-customizations.html).

Besides users signing themselves up, an administrator of any user pool can sign users up. The user then receives an
invitation to join the user pool. The following code snippet configures a user pool with relevant properties -

```ts
import { Duration } from '@aws-cdk/core';

new UserPool(this, 'myuserpool', {
  // ...
  adminSignUp: {
    tempPasswordValidity: Duration.days(4),
    invitationEmailSubject: 'Invite to join our awesome app!',
    invitationEmailBody: 'Hello {username}, you have been invited to join our awesome app! Your temporary password is {####}',
    invitationSmsMessage: 'Your temporary password for our awesome app is {####}'
  }
});
```

All email subjects, bodies and SMS messages support Cognito's message templating. Learn more about [message templates
here](https://docs.aws.amazon.com/cognito/latest/developerguide/cognito-user-pool-settings-message-templates.html).