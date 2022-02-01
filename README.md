# Deploy a Node.JS Application to Azure

This repository shows you how to use Okta in a Node.js application and how to deploy the application to Azure. Please read [Deploy a NodeJs application to Azure][blog] to see how it was created.

**Prerequisites:**

- [Node.js](https://nodejs.org/en/)
- [Azure Account](https://azure.microsoft.com)
- [Visual Studio Code](https://code.visualstudio.com/download)
- [VS Code Azure App Service Extension](https://marketplace.visualstudio.com/items?itemName=ms-azuretools.vscode-azureappservice)
- [Okta CLI](https://cli.okta.com)

> [Okta](https://developer.okta.com/) has Authentication and User Management APIs that reduce development time with instant-on, scalable user infrastructure. Okta's intuitive API and expert support make it easy for developers to authenticate, manage and secure users and roles in any application.

- [Getting Started](#getting-started)
- [Links](#links)
- [Help](#help)
- [License](#license)

## Getting Started

To run this example locally, run the following commands:

```bash
git clone https://github.com/nickolasfisher/Okta_Node_Azure.git
cd app
```

### Create an OIDC Application in Okta

Create a free developer account with the following command using the [Okta CLI](https://cli.okta.com):

```shell
okta register
```

If you already have a developer account, use `okta login` to integrate it with the Okta CLI.

Provide the required information. Once you register, create a client application in Okta with the following command:

```shell
okta apps create
```

You will be prompted to select the following options:

- Type of Application: **1: Web**
- Framework of Application: **Other**
- Redirect URI: `https://localhost:3000/authorization-code/callback`
- Post Logout Redirect URI: `https://localhost:3000`

The application configuration will be printed to `.okta.env`.

```dotenv
export OKTA_OAUTH2_ISSUER="{yourOktaDomain}/oauth2/default"
export OKTA_OAUTH2_CLIENT_SECRET="{yourClientSecret}"
export OKTA_OAUTH2_CLIENT_ID="{yourClientId}"
```

Create a new file in your project folder called .env Copy the values to there

```dotenv
OKTA_OAUTH2_ISSUER={yourOktaDomain}/oauth2/default
OKTA_OAUTH2_CLIENT_SECRET={yourClientSecret}
OKTA_OAUTH2_CLIENT_ID={yourClientId}
APP_BASEURL=
```

_Note: You will need to edit **APP_BASEURL** after deploying to Azure_

Use `npm run start` to run the application.

### Publish to Azure

Install the Azure App Service Extension

Log in to your Azure Account

Use **Ctrl + Shift + p** to open the command palette

Select **Azure App Service: Create New Web App...Advanced**

Give your application a new name

Create a new resource group
Give the resource group a name
Select Node 16 LTS for the runtime stack
Select Linux for the OS
Select the closest location to you for the resource location
Create a new App Service Plan
Select **Free (F1)**
Select **Skip for now** under application insights

Give Azure a few minutes to set up and a display will show up

Click **Deploy**

Wait a few minutes. After Azure is done it will display a link to you. Open the Azure Extension and open your app's `.env` file. Replace the empty value in your application with this one. Remember to remove the trailing slash.

Reload your site, your app should be loading.

### Finish Okta Setup

Navigate back to your Okta dashboard

Find the application you created for this project and click **Edit** under _General Settings_

Under _Login_, find the setting for _Sign-in redirect URIs_ and add the value for your app's base domain + `/authorization-code/callback`. For example, mine looks like this: `https://okta-azure.azurewebsites.net/authorization-code/callback`

Under signout redirect, add a value for your application's domain. Again, mine looks like this: `https://okta-azure.azurewebsites.net`

Navigate to your application, you should see the home page. Try to log in and see your profile then log out.

## Links

This example uses the following open source libraries from Okta:

- [Okta with NodeJs](https://developer.okta.com/code/nodejs/)
- [Okta OIDC Middleware](https://www.npmjs.com/package/@okta/oidc-middleware)
- [Okta CLI](https://github.com/okta/okta-cli)

## Help

Please post any questions as comments on the [blog post][blog], or visit our [Okta Developer Forums](https://devforum.okta.com/).

## License

Apache 2.0, see [LICENSE](LICENSE).

[blog]: https://developer.okta.com/blog/2021/xyz
