# Ballerina Microsoft SharePoint Termstore connector

[![Build](https://github.com/ballerina-platform/module-ballerinax-microsoft.sharepoint.termstore/actions/workflows/ci.yml/badge.svg)](https://github.com/ballerina-platform/module-ballerinax-microsoft.sharepoint.termstore/actions/workflows/ci.yml)
[![GitHub Last Commit](https://img.shields.io/github/last-commit/ballerina-platform/module-ballerinax-microsoft.sharepoint.termstore.svg)](https://github.com/ballerina-platform/module-ballerinax-microsoft.sharepoint.termstore/commits/master)
[![GitHub Issues](https://img.shields.io/github/issues/ballerina-platform/ballerina-library/module/microsoft.sharepoint.termstore.svg?label=Open%20Issues)](https://github.com/ballerina-platform/ballerina-library/labels/module%microsoft.sharepoint.termstore)

[Microsoft SharePoint Term Store](https://www.microsoft.com/en-us/microsoft-365/sharepoint/collaboration) is a centralized taxonomy management service within Microsoft 365 that enables organizations to define, organize, and govern managed metadata terms across sites and applications for consistent content classification.

The `ballerinax/microsoft.sharepoint.termstore` package offers APIs to connect and interact with the [Microsoft SharePoint Term Store API](https://learn.microsoft.com/en-us/graph/api/resources/termstore-store?view=graph-rest-1.0) endpoints, specifically based on [Microsoft Graph REST API v1.0](https://learn.microsoft.com/en-us/graph/api/resources/termstore-store?view=graph-rest-1.0).

## Setup guide

To use the Microsoft SharePoint Term Store connector, you must have access to the Microsoft SharePoint API through a [Microsoft Azure developer account](https://portal.azure.com/) and obtain an OAuth 2.0 access token by registering an application in Azure Active Directory. If you do not have a Microsoft account, you can sign up for one [here](https://signup.microsoft.com/).

### Step 1: Create a Microsoft Account and Set Up SharePoint

1. Navigate to the [Microsoft 365 website](https://www.microsoft.com/en-us/microsoft-365) and sign up for an account or log in if you already have one.

2. Ensure you have a **Microsoft 365 Business**, **Enterprise (E1, E3, or E5)**, or **SharePoint Online** plan, as access to the SharePoint Term Store API requires an active SharePoint Online subscription. The Term Store (Managed Metadata Service) is not available on personal or free-tier Microsoft accounts.

### Step 2: Register an Application and Generate an Access Token

1. Log in to the [Azure Portal](https://portal.azure.com/) using your Microsoft account credentials.

2. In the left-hand navigation menu, select **Azure Active Directory** (now called **Microsoft Entra ID**), then choose **App registrations** from the side menu.

3. Click **New registration**. Enter a name for your application, select the appropriate **Supported account types** (e.g., *Accounts in this organizational directory only*), and click **Register**.

4. Once the application is registered, navigate to **API permissions** in the left panel of your app registration. Click **Add a permission**, select **Microsoft Graph**, and then choose **Application permissions** or **Delegated permissions** depending on your use case. Search for and add the `TermStore.Read.All` and/or `TermStore.ReadWrite.All` permissions as required.

5. Click **Grant admin consent** for your organization to activate the permissions.

6. Navigate to **Certificates & secrets** in the left panel, then click **New client secret**. Provide a description and an expiry period, then click **Add**. Copy the generated **client secret value** immediately.

7. To obtain an access token, use your **Application (client) ID** and **Directory (tenant) ID** (both found on the app's **Overview** page) along with the client secret to authenticate against the Microsoft identity platform endpoint: `https://login.microsoftonline.com/{tenant-id}/oauth2/v2.0/token`.

> **Tip:** You must copy and store the client secret value somewhere safe. It won't be visible again in the Azure Portal after you navigate away from the page, for security reasons.

## Quickstart

To use the `Microsoft SharePoint Term Store` connector in your Ballerina application, update the `.bal` file as follows:

### Step 1: Import the module

```ballerina
import ballerinax/microsoft.sharepoint.termstore;
```

### Step 2: Instantiate a new connector

1. Create a `Config.toml` file and configure the obtained credentials:

```toml
clientId = "<Your_Client_Id>"
clientSecret = "<Your_Client_Secret>"
tenantId = "<Your_Tenant_Id>"
```

2. Create a `termstore:ConnectionConfig` and initialize the client:

```ballerina
configurable string clientId = ?;
configurable string clientSecret = ?;
configurable string tenantId = ?;

final termstore:Client termstoreClient = check new ({
    auth: <termstore:OAuth2ClientCredentialsGrantConfig>{
        clientId,
        clientSecret,
        tokenUrl: string `https://login.microsoftonline.com/${tenantId}/oauth2/v2.0/token`,
        scopes: ["https://graph.microsoft.com/.default"]
    }
});
```

### Step 3: Invoke the connector operation

Now, utilize the available connector operations.

#### Create a term store group

```ballerina
public function main() returns error? {
    termstore:MicrosoftGraphTermStoreGroup newGroup = {
        displayName: "Product Taxonomy Group",
        description: "Group for organizing product-related term sets"
    };

    termstore:MicrosoftGraphTermStoreGroup response = check termstoreClient->createGroup("contoso.sharepoint.com,site-id-here", newGroup);
}
```

### Step 4: Run the Ballerina application

```bash
bal run
```

## Examples

The `microsoft.sharepoint.termstore` connector provides practical examples illustrating usage in various scenarios. Explore these [examples](https://github.com/ballerina-platform/module-ballerinax-microsoft.sharepoint.termstore/tree/main/examples), covering the following use cases:

1. [Product taxonomy hierarchy builder](https://github.com/ballerina-platform/module-ballerinax-microsoft.sharepoint.termstore/tree/main/examples/product-taxonomy-hierarchy-builder) - Demonstrates how to build a structured product taxonomy hierarchy using the term store.
2. [Taxonomy migration setup](https://github.com/ballerina-platform/module-ballerinax-microsoft.sharepoint.termstore/tree/main/examples/taxonomy-migration-setup) - Illustrates how to configure and migrate existing taxonomy data into SharePoint term store.
3. [Cross termset synonym linking](https://github.com/ballerina-platform/module-ballerinax-microsoft.sharepoint.termstore/tree/main/examples/cross-termset-synonym-linking) - Demonstrates how to establish synonym relationships between terms across multiple term sets.
4. [Retire obsolete term sets](https://github.com/ballerina-platform/module-ballerinax-microsoft.sharepoint.termstore/tree/main/examples/retire-obsolete-term-sets) - Illustrates how to identify and retire outdated term sets from the SharePoint term store.

## Useful links

* For more information go to the [`microsoft.sharepoint.termstore` package](https://central.ballerina.io/ballerinax/microsoft.sharepoint.termstore/latest).
* For example demonstrations of the usage, go to [Ballerina By Examples](https://ballerina.io/learn/by-example/).
* Chat live with us via our [Discord server](https://discord.gg/ballerinalang).
* Post all technical questions on Stack Overflow with the [#ballerina](https://stackoverflow.com/questions/tagged/ballerina) tag.
