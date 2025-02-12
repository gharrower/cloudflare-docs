---
title: Azure guide
pcx_content_type: how-to
weight: 3
meta:
    title: Azure integration guide
---

# Azure integration guide

This tutorial will walk you through the steps for configuring a non-gallery enterprise application within Azure Active Directory to establish a SAML SSO connection with Area 1.

## 1. Azure Active Directory configuration

1. [Log in to Azure portal](https://portal.azure.com/) and open **Enterprise Applications**.

2. Select **New Application**.

    ![Create a new application](/email-security/static/sso/azure/step2-new-app.png)

3. Select **Non-gallery application**.

    ![Choose non-gallery as the type of application](/email-security/static/sso/azure/step3-non-gallery.png)

4. Give the application a descriptive name, and select **Add**.

    ![Give your application a descriptive name](/email-security/static/sso/azure/step4-add.png)

5. On the application **Overview** page, select **2. Set up Single Sign On**.

    ![Select single sign-on as the type of app](/email-security/static/sso/azure/step5-sso.png)

6. Select **SAML** as your single sign-on method.

7. Select the pencil icon to edit the **Basic SAML Configuration** as follows:
    | | |
    |------------------------------------------------|----------------------------------------------------|
    | **Identifier (Entity ID)**                     | `https://horizon.area1security.com`                |
    | **Reply URL (Assertion Consumer Service URL)** | `https://horizon.area1security.com/api/users/saml` |
    | **Sign-On URL**                                | Leave blank                                        |
    | **Relay State**                                | Leave blank                                        |
    | **Logout URL**                                 | Leave blank                                        |

8. Select **Save** to exit the Basic SAML Configuration.

9. Select the pencil icon to edit **SAML Signing Certificate** and make the following changes:

    - **Signing Option**: Select _Sign SAML response_ from the drop-down menu.
    - **Signing Algorithm**: Select _SHA-1_ from the drop-down menu.

    ![Select the pencil to make changes](/email-security/static/sso/azure/step9-saml-certificate.png)

10. Select **Save** to exit **SAML Signing Certificate**.

    ![Select Sign SAML response and SHA-1 from the menu](/email-security/static/sso/azure/step9-saml-signing-certificate.png)

11. Still in the **SAML Signing Certificate** section, find **Federation Metadata XML** and select **Download**. You will need this information for the SSO Configuration in the Area 1 dashboard.

    ![Download the Metadata XML information](/email-security/static/sso/azure/step11-download.png)

Your Azure configuration is now complete. It should look similar to this:

![Your Azure configuration should be similar to this one](/email-security/static/sso/azure/config-finished.png)

{{<Aside type="note">}}
Now that the application configuration is complete, update **User Assignments** and **Application Properties** as needed to ensure that authorized personnel are able to access the new application from their Apps Catalog. Additionally, you may choose to update the application logo image file or the privacy policy URL.
{{</Aside>}} 

## 2. Configure Area 1 to connect to Azure

1. Log in to the [Area 1 dashboard](https://horizon.area1security.com/).

2. Go to **Settings** (the gear icon).

3. In **Users and Actions** > **Users and Permissions** add the email addresses of all your authorized administrators.

    ![Fill out your authorized administrators](/email-security/static/sso/generic/step3-users-actions.png)

4. Go to **SSO Settings**, and enable **Single Sign On**.

    ![Enable SSO](/email-security/static/sso/generic/step4-sso.png)

5. In **SSO Enforcement**, choose one of the settings according to your specific needs:
{{<render file="_sso-enforcement.md">}}

6. For **SAML SSO Domain**, enter `login.microsoftonline.com`.

7. In **Metadata XML** paste the XML metadata you downloaded in the previous step 11. You can open the downloaded file with a text editor to copy all the text. Make sure there are no leading carriage returns or spaces when you copy the text. Your copied text should begin with:

    ```txt
    <?xml version="1.0" encoding="utf-8"?><EntityDescriptor ID="_<YOUR_DESCRIPTIOR_ID>" entityID="https://<YOUR_ENTITY_ID> " xmlns="urn:oasis:names:tc:SAML:2.0:metadata">...
    ```
8. Select **Update Settings** to save your configuration.

## 3. Test SSO configuration

After completing both the Azure and Area 1 set ups, you can test your SSO access. 
In this example, the logo for Area 1 has been updated.

{{<Aside type="note">}}Verify that the User Assignments and Application Properties of your new Azure AD application have been updated accordingly to ensure that authorized personnel are able to access the new application from their Office 365/Azure Apps Catalog page. Application logos and privacy policy URL can also be updated as needed.{{</Aside>}}

1. Log in to your [Office 365 portal](https://portal.office.com).

2. Select **All Apps**. 

3. Navigate to **Settings** > **SSO**.

4. Locate the Area 1 Horizon application (or whichever name you gave your application), and select it to initiate your SSO login with Area 1. 

5. If you configured everything correctly, you should be signed in to the Area 1 Portal and redirected to the dashboard.

## Troubleshooting

If you have trouble connecting your Azure account to Area 1, make sure that:

- The user exists in the Area 1 dashboard.
- The **Identifier** and **Reply URLs** in Azure AD are correct (refer to **Basic SAML Configuration** in step 7 of [Azure Active Directory configuration](#1-azure-active-directory-configuration)).
- **Sign SAML response** and **SHA-1** are selected in Azure AD (refer to **SAML Signing Certificate** in step 9 of [Azure Active Directory configuration](#1-azure-active-directory-configuration).
- The SAML SSO Domain is set correctly in the Area 1 dashboard (refer to step 6 in [Configure Area 1 to connect to Azure](#2-configure-area-1-to-connect-to-azure)).
- The name ID identifier is set to **Email Address**.

If all else fails, enable Chrome browser debug logs. Then, log your activity when SSO is initiated, and contact [Cloudflare support](https://support.cloudflare.com/hc/articles/200172476).