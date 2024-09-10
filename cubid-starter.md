# CUBID Starter App: Developer Technical Documentation
> NOTE: The CUBID Starter SDK still uses v1 or the CUBID APIs. Developers should take care in replacing these with v2, as described in the API section of these documents. The SDK will be updated and the v1 APIs will be depracated over time.

## Overview

The **CUBID Starter App** is built using the **Next.js** framework on top of **Supabase**. It inherits its structure from both **Create React App** and **Create Next App**, making it familiar to developers experienced with React and Next.js. The app demonstrates how to interact with CUBID APIs for digital identity management, including fetching unique user identifiers (UUIDs), trust scores, and detailed identity information.

The demo version of the app, which can be tested at [https://starter.cubid.me](https://starter.cubid.me/), is fully functional but does not store any user data, using Supabase only for authentication. This document provides technical guidance for developers on how to interact with the app and integrate CUBID APIs.


## Getting Started

### Prerequisites

1. Familiarity with **React** and **Next.js**.
2. Basic knowledge of **Supabase** for authentication.
3. Basic understanding of **API integration** and environment variables for secure key management.

### Running the App

To get started, developers can visit the live demo at [https://starter.cubid.me](https://starter.cubid.me/) and sign in using their email. Upon signing in, an OTP is sent to the email for verification. Once verified, developers can interact with the API demo page to see how CUBID API requests and responses work.

Alternatively, developers can fork the repository to run and customize the app locally. The open-source code can be found at [https://github.com/Cubid-Me/cubid-starter](https://github.com/Cubid-Me/cubid-starter).


## App Authentication Flow

The authentication flow in the demo app is handled through **Supabase**. The app uses **passwordless email-based authentication** via OTP (One-Time Password).

1. **Sign in with email:**
   - When the user signs in with their email, an OTP is sent to verify the email address.
   - After entering the OTP, the user is authenticated.

2. **Rate-limiting:**
   - **Note**: If you encounter an error while signing in, this could be due to rate-limiting. Wait for 2 hours before trying again.



## API Interaction

Once signed in, developers land on an API demo page that shows how to interact with the CUBID APIs. This page helps developers compose, send, and receive messages using the CUBID identity system.

### API Demonstration Flow

1. The page starts empty.
2. The green button, **Get UUID**, sends the user's email credential to CUBID and returns an **app-scoped UUID** and a boolean value indicating whether this was a **new user**.
3. The blue buttons trigger other API calls:
   - **Fetch Score**: Retrieves the user's overall trust score.
   - **Get Identity**: Returns detailed identity information.
   - **Get Score Details**: Fetches specific details about the user's identity stamps.

### 1. Get UUID

#### Endpoint: `/get_uuid`

The first action involves clicking the **Get UUID** button.

- **Request**: Sends the user’s email credential to CUBID.
- **Response**:
  - Returns a **UUID** (unique identifier) for the user.
  - Indicates if the user is new to the app (`New User Created`).

**Sample Response:**

```json
{
  "uuid": "cacfffb8-a683-4205-96e6-ab6b6c1a65cc",
  "new_user_created": false
}
```

- `uuid`: The app-scoped UUID assigned to the user.
- `new_user_created`: A boolean flag that shows whether the app has seen this user for the first time.

### 2. Fetch Score

#### Endpoint: `/fetch_score`

After obtaining the UUID, developers can click the **Fetch Score** button.

- **Request**: Fetches the user’s current score from CUBID.
- **Response**:
  - Returns a numerical score indicating the user’s trust score in the CUBID system.

**Sample Response:**

```json
{
  "score": 8
}
```

- `score`: The user's trust score, a measure of their credibility in the system.

### 3. Get Identity

#### Endpoint: `/get_identity`

This button retrieves identity details linked to the user's CUBID account.

- **Request**: Asks for detailed identity information.
- **Response**:
  - Provides a list of identity stamps (email, phone, Gitcoin passport, Google, etc.).

**Sample Response:**

```json
{
  "score_details": [
    {"near": "harrydhillon.near"},
    {"phone": "918288807134"},
    {"google": "harjaapdhillon.hrizn@gmail.com"},
    {"gitcoin_passport": "0x712913366F98aa0A5E77EfddA05BaC65C52ae9bD"},
    {"email": "harvydhillon16@gmail.com"},
    {"evm": "0x712913366F98aa0A5E77EfddA05BaC65C52ae9bD"}
  ]
}
```

- `score_details`: A detailed list of identity stamps collected by the user. This could include their email, phone number, NEAR protocol identity, Ethereum (EVM) address, and more.

### 4. Get Score Details

#### Endpoint: `/get_score_details`

This endpoint provides more granular information about the user’s score.

- **Request**: Asks for details about how the user’s score is calculated.
- **Response**:
  - Returns a breakdown of how the user's identity stamps contribute to their overall score.

**Sample Response:**

```json
{
  "score": [
    {}, {"phone": 2}, {}, {"google": 1}, {}, {"email": 1}, {}, {"google": 1}, {"phone": 2}, {}, {"email": 1}, {}
  ]
}
```

- `score`: A detailed breakdown of each identity stamp’s contribution to the overall score.


## Key Considerations

### API Key Management

The demo app currently displays the **API key** on the page for simplicity. However, in a production app, the API key should be securely stored using **environment variables**. This will prevent unauthorized access to sensitive data.

For example, in **Next.js**, you can store environment variables like this:

```bash
NEXT_PUBLIC_API_KEY=your_api_key_here
```

Then use it in your code as:

```javascript
const apiKey = process.env.NEXT_PUBLIC_API_KEY;
```

### Data Privacy and User Trust

While the demo exposes technical details of the API for educational purposes, in a production environment, developers should show users **only the necessary information**. Displaying detailed identity data could help build user trust by ensuring transparency about what data is collected.

For example, you could show users a simple summary:

```
Data collected:
- Email: Verified
- Phone: Verified
- Google Account: Linked
```

This level of transparency can enhance trust in the CUBID ecosystem.


## Next Steps

Once you are familiar with the demo and how the CUBID APIs work, you can:
1. **Customize the app** for your specific needs.
2. **Integrate CUBID APIs** into your app’s authentication or identity verification flows.
3. Ensure secure storage of **API keys** and maintain **user privacy** by not exposing sensitive data unnecessarily.


Here’s the rewritten final section, incorporating both pieces of information:

---

## Forking the Repository and Getting Started

Follow these steps to fork the repository, set up Supabase and CUBID, and deploy your project:

### 1. Fork the Repository

Start by forking the CUBID Starter repository to your own GitHub account and then clone it to your local machine. This will give you a copy of the project that you can customize and develop.

- **Repository link**: [https://github.com/Cubid-Me/cubid-starter](https://github.com/Cubid-Me/cubid-starter)

```bash
git clone https://github.com/<your-username>/cubid-starter
cd cubid-starter
```

### 2. Set Up Your Supabase Project

Create a new project in **Supabase**:

- Go to [Supabase](https://supabase.io) and create a new project.
- Once your project is set up, get your **Supabase URL** and **anon key** from the dashboard. You will use these for your local environment and in your deployment settings.

### 3. Sign Up for a Cubid App

- Visit the **CUBID Admin** at [Cubid Admin](https://admin.cubid.me) and sign up for your app.
- Obtain your **CUBID API keys** from the Cubid dashboard. You will need these keys to interact with the Cubid APIs.

### 4. Add Supabase and CUBID Keys to Environment Variables

For local development, create a `.env.local` file in the root of your project and add your Supabase and Cubid keys:

```bash
NEXT_PUBLIC_SUPABASE_URL=<your-supabase-url>
NEXT_PUBLIC_SUPABASE_ANON_KEY=<your-supabase-anon-key>
NEXT_PUBLIC_CUBID_API_KEY=<your-cubid-api-key>
```

### 5. Deploy on Vercel or Netlify

You can easily deploy your project on **Vercel** or **Netlify**. These platforms seamlessly integrate with Next.js, making deployment straightforward.

- After deployment, add your **Supabase** and **CUBID** keys to the environment variables in your deployment settings on Vercel or Netlify.
  
For example, in **Vercel**, go to your project’s dashboard, navigate to **Settings** > **Environment Variables**, and add:

- `NEXT_PUBLIC_SUPABASE_URL`
- `NEXT_PUBLIC_SUPABASE_ANON_KEY`
- `NEXT_PUBLIC_CUBID_API_KEY`

### 6. Sign In and Experiment with CUBID APIs

Once your app is deployed:

- Sign in using email-based authentication and explore the interface.
- Test the CUBID APIs to understand how to fetch UUIDs, trust scores, and identity information. Experimenting with these APIs will give you a deeper understanding of the integration.

### 7. Start Developing

Now that your environment is set up:

- Start developing your project by adding new features, fixing bugs, and improving the codebase.
- Utilize the **documentation** and resources available for **Supabase** and **CUBID** to assist in your development process.

Feel free to reach out to the community or refer to the repository’s README for additional guidance as you explore and extend the functionality of your app.