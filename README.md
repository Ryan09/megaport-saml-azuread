# Megaport SSO with Entra ID (Azure AD) SAML

This article provides a step by step guide to configure SSO in to the Megaport Portal from Entra ID, using SAML.

## Introduction

Single Sign On (SSO) allows a central Identity Provider (IdP) to control authentication in to multiple applications or Service Providers (SP), avoiding the requirement for users to manage multiple sets of credentials or sign in multiple times.

SAML is an open standard for implementing SSO between IdPs and external applications/SPs.

The Megaport Portal supports SAML 2.0 as a way to configure SSO from Microsoft Entra ID (formerlly called Azure AD), as well as other IdPs.

## Prerequisites

To complete the process you'll need the following.

### Megaport Portal

You will need a Megaport Portal account and to be signed in with [Company Admin](https://docs.megaport.com/portal-admin/user-management/roles/) permissions.

Megaport's documentation on configuring SAML for SSO is available at <https://docs.megaport.com/setting-up/enforcing-sso/>

### Entra ID (Azure AD)

You will need an Entra ID (Azure AD) tenancy, and to be signed in to the Entra Admin Center as a user with the Cloud Application Administrator or Application Administrator role.

## Process

This section details the steps required to configure SSO.

There are three steps:

1. [Collect URIs from Megaport Portal](#1-collect-uris-from-megaport-portal)
2. [Create Enterprise application in Entra ID](#2-create-enterprise-application-in-entra-id)
3. [Configure SAML in Megaport Portal](#3-configure-saml-in-megaport-portal)

### 1. Collect URIs from Megaport Portal

The first step is to collect the integration URIs form the Megaport Portal.

In the Megaport Portal, go to the Company menu and select Security Settings.

![Megaport Security Settings](megaport-security-settings.png)

You will need the Audience URI (Entity ID) and the IdP Response URL (Reply URL).

### 2. Create Enterprise application in Entra ID

In the Entra Admin Centre, go to Manage > Enterprise applications, and click New Application.

![Entra Admin Center](entra-admin-center.png)

On the Browse Microsoft Entra Gallery page, choose Create your own application.

![Browse Entra Gallery](browse-entra-gallery.png)

Enter a name for the application (eg. Megaport Portal), and choose "Integrate any other application you don't find in the gallery (Non-gallery)", then click Create.

Once the application has been created, click "Get started" on the "Set up single sign on" tile.

![Set up single sign on](set-up-single-sign-on.png)

Select SAML, then click the Edit button to the right of the Basic SAML Configuration section.

Enter the URIs from the Megaport Portal. The Audience URI id the "Identifier (Entity ID)", and the IdP Response URL is the "Reply URL (Assertion Consumer Service URL)".

Save the changes using the Save button at the top of the panel.

![alt text](image-4.png)

In the SAML Signing Certificate section, copy the App Federation Metadata Url. This will be required in the next step in the Megaport Portal.

![alt text](image-5.png)

### 3. Configure SAML in Megaport Portal

The final step is to configure SAML in the Megaport Portal.

Return to the Security Settings section, and click "Add SAML Connection".

![alt text](image-6.png)

Fill in the fields as detailed below.

- **Provider Name:** Name for this SAML provider configuration, lowercase, numbers, and dashes only.
- **Domains:** The domains of the email addresses of the users which will be using this SSO configuration.
- **Metadata URL:** The "App Federation Metadata Url" copied from the Entra portal in the previous section.
- **Attribute mapping:** The Megaport Portal uses email adresses as user names, so the 'email' attribute must be mapped to a field in the SAML configuration that contains the email address which matches the user name in the Megaport Portal. The SAML attributes must include the full path. For a typically Entra ID configuration the email attribute should be mapped to `http://schemas.xmlsoap.org/ws/2005/05/identity/claims/emailaddress` but this can be customised if required.

![alt text](image-7.png)

Click Save

The configuration is now complete, and attempting to log in to the Megaport Portal using an email address in the domains configured above should redirect to the Microsoft sign in workflow.

## Additional Information

This section provides some additional context, links, and information.

### User Provisioning and Permissions

Automatic user provisioning (SCIM or JIT) is not currently supported. User accounts should be created in the Megaport Portal as normal, with the appropriate Role assigned.

More information on creating users is available at <https://docs.megaport.com/portal-admin/user-management/>

Information on user Roles and permissions is available at <https://docs.megaport.com/portal-admin/user-management/roles/>

#### Inviting users

When SSO is enabled, newly invited users will receive an email with a link to activate their account. The link must be clicked before they are able to log in to the Portal.

### Enforcing SSO

The Portal can be configured to require users to log in using SSO. This allows you to prevent access to the Portal other than through your Identity Provider. This can be enabled under Company > Security Settings by setting Enforce SSO to ON.

Instructions for enforcing SSO are available at <https://docs.megaport.com/setting-up/enforcing-sso/?h=sso#making-sso-mandatory-for-users>

Users with the Company Admin Role are always allowed to log in locally. This is to allow break glass access in the event that there is a problem with SSO.

### IdP Initiated SignOn

IdP Initiated SignOn is not currently supported, only Service Provider Initiated SignOn is supported. This means users will have to log in via <https://portal.megaport.com>. Attempting to sign in with an IdP Initiated flow will result in a `relayState` error.

### Other troubleshooting and FAQ

More info is available in the Megaport Documentation page at <https://docs.megaport.com/troubleshooting/sso-faq/>
