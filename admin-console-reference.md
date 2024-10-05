# Admin Console Reference

## **Authenticate & Login**
Developers can access the Admin Console [here]](https://admin.cubid.me).

Like all CUBID sites, there is no registration or password required. Instead we leverage one-time passwords. Enter your email and get a one-time-passcode via email. Then enter the passcode into the prompt and that's it.

### Empty Dashboard
There is no registration required. If you log in for the first time you are greeted by an empty screen and a message "No Apps Found". Returning users can immediately access and continure configuring their apps. There are also very few "Save" buttons. Instead, changes take effect immediately.

## **Create App** 
### Step 1: Setup
The App creation process is streamlined with the following simple steps:
- Click Create App
- Enter Details
- Click Next to enter Page Setting

Settings:
- **App Name**: Only used for your own identification (not shown to users)
- **App URL**: Full path to where your app is hosted, e.g. https://app.acme.com
- **Schema**: Choice depends on if you are planning on allowing Gitcoin Passport or not.
  - Enable Gitcoin Passport import (recommended for web3 apps). Some of your users may already have created their identity on Gitcoin Passport, which they can import to CUBID. In this case, select **Complementing Gitcoin Passport**
  - Dont enable Gitcoin Passport import (recommended for web2 and legacy apps). If your users are not native web3 users, then choose this option. The user experience will be much simpler and smoother, with no requirement to user 3rd party wallets and 3rd party platforms. In this case, select **Emulating Gitcoin Passport**
  - Custom schema: Contact us directly if you need your own custom scoring schema.

### Step 2: Add Allow-pages
Allow-pages are how your users interact with CUBID from within your app, either using iFrames or redirects. A Common scenario is to send new users to a CUBID Allow Page as part of your user onboarding process. In this case you will likely only need one Allow Page. We also support sequential / progressive onboarding, whereby users are asked to add more and more information during their journey in your app. In this case you will likely need several Allow Pages.

- **Add Page**: Adds a new page in the listing of Pages.
- **Input Field**: Name your page for easy identification.
- **Remove**: This button removes the corresponding Allow Page.
- **Redirect URL**: Each page has a separate Redirect, to which users are sent after they are done entering data. 
- **Stamp Type**: Stamps are verifiable identity attributes. You can select which Stamps you want CUBID to collect from your users.

**Requested Info**: We support different levels of detail / granularity you can request for each Stams. The granularity can be different on different Allow Pages, for example to allow you to ask a less intrusive questions up front, but still be able to ask for more information later, when needed.
- **Not Included**: Default. The information will not be requested or even mentioned on the Allow Pages. You will not know if the user has this information registered with CUBID or not.
  - Use case 1: You are not intereted in this data point, and therefore also don't include it in the score calculation.
  - Use case 2: You want to include the data point in the Score calculation, but you don't need to know if it was actually included or not.
- **Hashed Value**: Least intrusive way of asking. The user will not provide you with their data, but will allow you to know about the existence of their data. The typical use case for this is to include their data iWe will hash the data and send it to you if you wish. You can use this to 
  - Use case 3: Count the number of data points the users has connected (e.g. a user has three different wallets)
  - Use case 4: You can include the data point in the Score calculation and see the individual score for this data point.
  - Use case 5: You can compare the hash of this user vs the hash of another, to ensure that the same identity hasn't been user more than once. (This is also how the Sybil protection algo of Gitcoin Passport works.)
- **Approximate**: To be introduced in v2. Will allow your app to see only the area code of a phone number, geolocation within 1km, the last 4 digits of an public key, etc.
- **Identity**: You can query CUBID to get the unique identifier of a stamp, such as the unencrypted email address or phone number of the user, or the public key of their crypto account. This option is intrusive and should only be used if you can justify to your user why you need it.
  - **Full JSON**: Most intrusive. You can query CUBID to get accesss to the full JSON object representing the user, including when they created and validated the stamp, and any metadata provided to us from 3rd parties (e.g. social networks / OAUTH providers) during our validation process. This option is typically NOT recommended and should be used very sparingly. Your users WILL questions you why you are asking for so much details.
 
 Save configurations, and your new App is now ready to interact with the various CUBID APIs.

## **The Dashboard**
Your app should now be available on the Dashboard, with the following attributes:
- **App Name**: The name you gave your App.
- **App ID**: Used in API calls to CUBID.
- **Secret Key**: Used in API calls to CUBID.
- **Rotate API Keys**: It's a good practice to rotate your API keys regularly. Press this button to immediately invalidate the old key and generate a new one.
- **Delete App**: Removes all configuration related to your app. 
  - Note: This does not remove the user data that was gathered from CUBID. As a reminder, this data belongs to the user themselves and can only be deleted by them.
  - Caution: This action is permanent and **will make it impossible for you to access this data**, without first asking your users to re-share it. 

Similarly, you can access your Allow-pages related to each app here:
- **Page Name**: The name you gave your App.
- **Page Link**: Reference this when creating your iFrames or Redirect to the page, along with the user identifier.
  - Example: https://allow.cubid.me/123e4567-e89b-12d3-a456-426614174000?uid=f12e4567-e89b-42d3-a456-326614174bbb
- **Edit Page**: Click the button to edit your page and the stamp details you want to ask the user for.
- **Delete Page**: Removes all configuration related to your page. You still have access to all the collected data, and can set up a new page to replace the old one (though the page id will be different).
