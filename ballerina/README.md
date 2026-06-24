## Overview

[Microsoft SharePoint](https://www.microsoft.com/en-us/microsoft-365/sharepoint/collaboration) is a collaborative platform that enables organizations to manage content, knowledge, and applications to empower teamwork, quickly find information, and seamlessly collaborate across the enterprise.

The `ballerinax/microsoft.sharepoint.termstore` package offers APIs to connect and interact with the [Microsoft SharePoint Term Store API](https://learn.microsoft.com/en-us/graph/api/resources/termstore-store?view=graph-rest-1.0) endpoints, specifically based on [Microsoft Graph REST API v1.0](https://learn.microsoft.com/en-us/graph/api/resources/termstore-store?view=graph-rest-1.0).

## Setup guide

To use the Microsoft SharePoint Term Store connector, you must have access to the Microsoft SharePoint API through a [Microsoft Azure developer account](https://portal.azure.com/) and obtain an OAuth 2.0 access token by registering an application in Azure Active Directory. If you do not have a Microsoft account, you can sign up for one [here](https://signup.microsoft.com/).

### Step 1: Create a Microsoft Account and Set Up SharePoint

1. Navigate to the [Microsoft 365 website](https://www.microsoft.com/en-us/microsoft-365) and sign up for an account or log in if you already have one.

2. Ensure you have a Microsoft 365 Business or Enterprise plan (such as Microsoft 365 Business Standard, Business Premium, E3, or E5), as access to SharePoint Term Store and its API capabilities is restricted to users on these plans.

### Step 2: Register an Application and Generate an Access Token

1. Log in to the [Azure Portal](https://portal.azure.com/) using your Microsoft account credentials.

2. In the left-hand navigation menu, select **Azure Active Directory** (or search for it in the top search bar).

3. Under **Manage**, select **App registrations**, then click **New registration**.

4. Provide a name for your application, select the appropriate **Supported account types** (e.g., single tenant or multitenant), and click **Register**.

5. Once the application is registered, navigate to **API permissions** under **Manage**. Click **Add a permission**, select **Microsoft Graph**, and add the required permissions for SharePoint Term Store (e.g., `TermStore.Read.All` or `TermStore.ReadWrite.All`). Click **Grant admin consent** to approve the permissions.

6. To generate a client secret, navigate to **Certificates & secrets** under **Manage**, click **New client secret**, provide a description and expiry period, and click **Add**. Copy the generated secret value immediately.

7. Note your **Application (client) ID** and **Directory (tenant) ID** from the **Overview** page, as these are required along with the client secret to obtain an OAuth 2.0 access token for authenticating API requests.

> **Tip:** You must copy and store the client secret value somewhere safe. It won't be visible again in the Azure Portal after you navigate away from the page, for security reasons.

## Quickstart

To use the `microsoft.sharepoint.termstore` connector in your Ballerina application, update the `.bal` file as follows:

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
        description: "Group for managing product-related terms"
    };

    termstore:MicrosoftGraphTermStoreGroup response = check termstoreClient->createGroup("contoso.sharepoint.com,abc123,def456", newGroup);
}
```

### Step 4: Run the Ballerina application

```bash
bal run
```

## Examples

The `microsoft.sharepoint.termstore` connector provides practical examples illustrating usage in various scenarios. Explore these [examples](https://github.com/ballerina-platform/module-ballerinax-microsoft.sharepoint.termstore/tree/main/examples), covering the following use cases:

1. [Product taxonomy hierarchy builder](https://github.com/ballerina-platform/module-ballerinax-microsoft.sharepoint.termstore/tree/main/examples/product-taxonomy-hierarchy-builder) - Demonstrates how to build a structured product taxonomy hierarchy using the Ballerina connector for Microsoft SharePoint Term Store.
2. [Taxonomy migration setup](https://github.com/ballerina-platform/module-ballerinax-microsoft.sharepoint.termstore/tree/main/examples/taxonomy-migration-setup) - Illustrates how to configure and migrate existing taxonomy data into a SharePoint Term Store.
3. [Cross termset synonym linking](https://github.com/ballerina-platform/module-ballerinax-microsoft.sharepoint.termstore/tree/main/examples/cross-termset-synonym-linking) - Demonstrates how to establish synonym relationships between terms across multiple term sets.
4. [Retire obsolete term sets](https://github.com/ballerina-platform/module-ballerinax-microsoft.sharepoint.termstore/tree/main/examples/retire-obsolete-term-sets) - Illustrates how to identify and retire outdated term sets from the SharePoint Term Store.
