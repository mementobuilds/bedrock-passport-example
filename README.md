# Orange ID by Bedrock - React (NPM) Integration Guide

<!--toc:start-->

- [Bedrock passport integration example](#bedrock-passport-integration-example)
  - [How to Install](#how-to-install)
  - [How to integrate](#how-to-integrate)
    - [Configuration Options](#configuration-options)
  - [How to use](#how-to-use) - [Styling available for the panel](#styling-available-for-the-panel) - [Configuration Options](#configuration-options) - [example of using the user hook, check if user is logged in](#example-of-using-the-user-hook-check-if-user-is-logged-in)
  <!--toc:end-->

## How to Install

```bash
npm i @bedrock_org/passport
```

## How to integrate

Wrap your application or router with the bedrock-passport provider

```jsx

import { BedrockPassportProvider } from "@bedrock_org/passport";
...
const Provider: React.FC<ProviderProps> = ({ children }) => {
  return (
    <BedrockPassportProvider
      baseUrl="https://api.bedrockpassport.com" // Base API URL – no need to change this. Leave as is.
      authCallbackUrl="https://yourdomain.com/auth/callback" // Replace with your actual callback URL
      tenantId="orange-abc123"  // Your assigned tenant ID - you can request one at https://vibecodinglist.com/orange-id-integration
    >
      {children}
    </BedrockPassportProvider>
  );
};

```

### Configuration Options

| Parameter         | Description                                                 |
| ----------------- | ----------------------------------------------------------- |
| `baseUrl`         | The Bedrock Passport API base URL                           |
| `authCallbackUrl` | The URL where users will be redirected after authentication |
| `tenantId`        | Your Bedrock Passport tenant identifier                     |
| `walletConnectId` | Your WalletConnect project ID for wallet connections        |

Create the callback page for handling the token from server

```jsx
function AuthCallback() {
  const { loginCallback } = useBedrockPassport();

  useEffect(() => {
    const login = async (token: string, refreshToken: string) => {
      const success = await loginCallback(token, refreshToken);
      if (success) {
        //redirect to the page you want
        window.location.href = "/";
      }
    };

    const params = new URLSearchParams(window.location.search);

    const token = params.get("token");
    const refreshToken = params.get("refreshToken");

    if (token && refreshToken) {
      login(token, refreshToken);
    }
  }, [loginCallback]);

  return <div>Signing in...</div>;
}
```

## How to use

Once you wrapped the application with the provider,
you can start to add in the component and hooks for where you need

```jsx

import { useBedrockPassport, LoginPanel, Button } from "@bedrock_org/passport";
import "@bedrock_org/passport/dist/style.css";

...
<LoginPanel
  panelClass="container p-2 md:p-8 rounded-2xl max-w-[480px]"
  buttonClass="hover:border-orange-500"
  headerClass="justify-center"
  title="Sign in to"
  titleClass="text-xl font-bold"
  logo="https://irp.cdn-website.com/e81c109a/dms3rep/multi/orange-web3-logo-v2a-20241018.svg"
  logoAlt="Orange Web3"
  logoClass="ml-2 md:h-8 h-6"
  showConnectWallet={false}
  walletButtonText="Connect Wallet"
  separatorText="OR"
  separatorTextClass="bg-orange-900 text-gray-500"
  separatorClass="bg-orange-900"
  linkRowClass="justify-center"
/>

```

Use tailwind variable for the class customization: refer to [tailwindcss](https://tailwindcss.com/docs)

[Example Integration](https://github.com/rfl-nftplatform/bedrock-passport-example)

The library is still in development, more features will be added in the future.

### Styling available for the panel

### Configuration Options

| Parameter            | Description                                                  |
| -------------------- | ------------------------------------------------------------ |
| `panelClass`         | panel styling                                                |
| `buttonClass`        | all button styling                                           |
| `headerClass`        | header styling                                               |
| `title`              | title of the panel                                           |
| `titleClass`         | title styling                                                |
| `logo`               | logo url                                                     |
| `logoAlt`            | logo alt string                                              |
| `logoClass`          | logo styling                                                 |
| `showConnectWallet`  | boolean, false by default                                    |
| `walletButtonText`   | string in wallet button text                                 |
| `walletButtonClass`  | wallet button styling, by default will follow button styling |
| `separatorText`      | text in the middle of separator                              |
| `separatorTextClass` | separator text styling                                       |
| `separatorClass`     | separator styling                                            |

### example of using the user hook, check if user is logged in

```jsx
const { isLoggedIn } = useBedrockPassport();
```


## 📦 User Object Returned After Login

When a user logs in successfully using Bedrock Passport, you can access a structured user object like the following:

```json
{
  "id": "clf8138x000045o01a4p2ajpi",
  "email": "testuser@example.com",
  "name": "Test User",
  "displayName": "testname",
  "userName": "",
  "address": "",
  "country": "",
  "company": "",
  "phoneNumber": "",
  "bio": "Just a sample user bio.",
  "role": "user",
  "playfabId": "9854EA000727F7A1",
  "sessionTicket": "",
  "picture": "https://cdn.example.com/images/sample_user.png",
  "banner": "https://cdn.example.com/images/sample_user_banner.png",
  "ethAddress": "0x1234567890abcdef1234567890abcdef12345678",
  "waxAddress": "",
  "photoUrl": "https://cdn.example.com/images/sample_user.png",
  "createdAt": "2025-04-07T06:21:36.674Z",
  "provider": "google"
}
```

### Explanation of Fields

| Key             | Description                                                                                       |
|------------------|---------------------------------------------------------------------------------------------------|
| `id`             | Unique Orange ID – the main identifier for the user.                                             |
| `email`          | User's email address.                                                                            |
| `name`           | User’s full name (from the provider).                                                            |
| `displayName`    | Custom display name editable at [vibecodinglist.com/profile](https://vibecodinglist.com/profile) |
| `userName`       | Optional username (currently unused).                                                            |
| `address`        | General address field (optional).                                                                |
| `country`        | Country of the user (optional).                                                                  |
| `company`        | Company name (optional).                                                                         |
| `phoneNumber`    | User’s phone number (optional).                                                                  |
| `bio`            | Short biography set by the user.                                                                 |
| `role`           | User role (e.g., `user`, `admin`, etc.).                                                         |
| `playfabId`      | Internal ID for PlayFab integration – can be ignored.                                            |
| `sessionTicket`  | PlayFab session ticket – can be ignored.                                                         |
| `picture`        | User's profile image URL. Can be used for avatars.                                               |
| `banner`         | User's banner image URL.                                                                         |
| `ethAddress`     | User’s Ethereum wallet address (if connected).                                                   |
| `waxAddress`     | User’s WAX wallet address (if connected).                                                        |
| `photoUrl`       | **Deprecated** – replaced by `picture`.                                                          |
| `createdAt`      | Timestamp when the user account was created.                                                     |
| `provider`       | Authentication provider used (e.g., `google`, `apple`, `wallet`).                               |



## ⚙️ Features Configuration

You can customize which login methods to show by passing a `features` object to the `<LoginPanel>` component:

### Example (React / NPM):

```tsx
<LoginPanel
  ...
  features={{
    enableWalletConnect: true,
    enableAppleLogin: true,
    enableGoogleLogin: true,
    enableEmailLogin: true,
  }}
/>
```

### Example (HTML / Script Tag):

```js
React.createElement(Bedrock.LoginPanel, {
  ...
  features: {
    enableWalletConnect: true,
    enableAppleLogin: true,
    enableGoogleLogin: true,
    enableEmailLogin: true,
  }
})
```

### Available Feature Flags

| Feature                | Description                              |
|------------------------|------------------------------------------|
| `enableWalletConnect`  | Enables Web3 wallet login                |
| `enableAppleLogin`     | Enables Apple ID login                   |
| `enableGoogleLogin`    | Enables Google login                     |
| `enableEmailLogin`     | Enables email-based login (magic link)   |
