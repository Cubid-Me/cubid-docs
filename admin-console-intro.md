# CUBID' Admin Console

## **Overview**

The [**CUBID Admin Console**](https://admin.cubid.me)  our platform for developers and business owners to efficiently create, configure, and manage their applications within the CUBID ecosystem. It simplifies the management of user authentication, API keys, and identity verification using CUBID's Sybil-resistant protocol. In the console, admins can also configure which **Cubid Stamps** they want to collect from users, either as required or as optional. Stamps are the verifiable identity attributes that the App will use. Developers can also choose a **Scoring Schema** for user trust scores based on collected identity stamps, set up their own schema for custom requirements.

This document outlines the technical aspects of the [CUBID Admin Console](https://admin.cubid.me), including its key features, functionality, and integration processes.

## **Key Features**

### 1. App Creation and Configuration
   The [CUBID Admin Console](https://admin.cubid.me) enables developers to create new apps within the CUBID ecosystem easily. Using it, developers can:
   - Specify their App name. It is also possible to set up multiple Apps, for exmple dedicated Apps for sandbox, test, and production.
   - Identif App domain and other parameters.

### 2. API Key Management
   The console provides automatic provisioning and management of API keys, allowing developers to:
   - Generate new API keys and associated secrets.
   - API keys can be generated or revoked directly from the console. Old API keys are automatically invalidated as new ones are generated, ensuring only current keys are usable.
   - Manage key permissions based on App-specific requirements.
   - Admins can copy keys for easy integration into their apps.

### 3. Authentication Management
   The Admin Console supports secure, passwordless access using **email-based OTPs** (and **Passkey webAuthn** (FIDO2) is coming soon too). This ensures that only authorized admins can configure and manage the App.

### 4. Managing Stames & Identities: Cubid Stamp Configuration
   Admins can define which Cubid Stamps are required, optional, or used for user authentication:
   - **Required Stamps**: These will be mandatory for new user sign-ups.
   - **Optional Stamps**: Users can opt-in to provide these, perhaps in order to enhance their trust score.
   - **Authentication Stamps**: Stamps used to authenticate users (e.g., email, phone, wallet).
   - This configuration directly impacts what is displayed on the various **AllowPages** (which is where end users complete their identity verifications).


### 5. Custom Scoring Schemas
   The console allows admins to use default scoring schemas or clone and customize them. 
   - Three standard schemas available out of the box 
   - Developers can assign weights to different stamps and define custom metrics for trust scores, if they have custom requirements.

### 6. Admin Role Management
   Admins can assign other users as administrators for the App. This includes:
   - Adding and removing admins.
   - Configuring admin permissions and managing access control.

### 7. Backend Services Integration
   The [Admin Console]](https://admin.cubid.me) integrates with various backend services to streamline App management:
   - **User Authentication Service**: Handles OTP generation, validation, and Passkey setup.
   - **Key Management Service**: Ensures secure storage and rotation of API keys.
   - **App Management Service**: Automates App setup and updates stamp configurations.


## **Workflow**
The App creation process is streamlined with the following steps:
- **Step 1**: Enter the App's name and domain.
- **Step 2**: Configure a scoring schema or choose a default one.
- **Step 3**: Create and define which Allow-pages you need for your app
- **Step 4**: Select Cubid Stamps for user authentication and scoring.
- **Step 5**: Save configurations, and the App is ready to interact with CUBID APIs.
- **Step 6**: Copy the API keys that are automatically generated for the App.
- **Step 7**: Use the provided API keys, endpoints, hooks and allos-pages within your own App configuration.


## **Security Features**

- **Passwordless Authentication**: The Admin Console relies on email-based OTPs for secure login, reducing the risk of password-related breaches.
- **Rate Limiting**: OTP requests are rate-limited to prevent abuse.
- **Encrypted API Keys**: API keys and secrets are securely stored using AES encryption.
- **HTTPS**: All communications are conducted over HTTPS to prevent man-in-the-middle attacks.


## **Technical Architecture**

### Frontend
- **Framework**: Built with **Next.js** for Server-Side Rendering (SSR) and enhanced performance.
- **Authentication**: Integrated with backend services for OTP management.

### Backend
- **Programming Language**: **Node.js** to ensure compatibility with the frontend and efficient handling of authentication and management tasks.
- **Database**: **PostgreSQL** for securely storing App configurations, user data, and API keys.
- **Security**: All sensitive data - inclusive of all user data - within CUBID is encrypted at rest and is further protected with row-level security rules. 


## **Integration with CUBID APIs**

The [Admin Console](https://admin.cubid.me) is built to interact seamlessly with the CUBID APIs, making it easier for developers to integrate Sybil resistance and identity verification into their apps. Key integration points include:

1. **API Key Generation**: Use the console to generate keys that will be required for accessing various CUBID services.
   
2. **UUID Retrieval**: The App can request a UUID for users based on unique identifiers (email, phone, wallet).

3. **Trust Score Retrieval**: Use the `_getTrustScore` API to retrieve a user's trust score based on the configured stamps.

4. **Stamp Verification**: The console allows apps to define what stamps are necessary, and developers can use the API to verify if users have provided the required stamps.


## **Future Roadmap**

- **Passkey Management**: FIDO2-style webAuthn passkeys, along with 2FA, are coming soon across the whole CUBID ecosystem of Apps, including the Admin Console.
- **Custom Schema Builder**: CUBID already supports custom schemas, with support from our team. We are planning to build a self-serve interface for developers who want to build their own custom scoring schema.  
- **Encryption Enhancements**: Quantum-proof end-to-end encryption - in transit and at rest - is currently being evaluated.  
- **Domain Management**: In future updates, admins will be able to purchase and manage domains directly from the console.
- **User Acquisition Tools**: Analytics and marketing tools such as email lists will be introduced to help apps track user growth and engagement.
- **Expanded Login Support**: Future releases will support OAuth, social logins, and additional authentication methods for end users.


## **Conclusion**

The [CUBID Admin Console](https://admin.cubid.me) is a powerful tool for developers seeking to implement robust identity management and Sybil defense mechanisms into their apps. By simplifying App setup, authentication management, and trust scoring, the console allows developers to focus on their core application while leveraging the power of the CUBID ecosystem for secure and privacy-preserving user verification.

For more information or to get started, visit [cubid.me](https://cubid.me) or access the **API documentation**.


**Contact Information**
- **Support Email**: support@cubid.me
- **Console**: [CUBID's Admin Console](https://admin.cubid.me)
- **Website**: [cubid.me](https://admin.cubid.me)