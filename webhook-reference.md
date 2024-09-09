# Webhook Reference: Event Types

Below are the detailed descriptions of the webhooks available in CUBID.



## 1. Blacklisted Stamp (Sybil Attack Detected)

**Trigger**: When a user’s stamp is blacklisted, either due to suspicious activity, account compromise, or Sybil attack detection. This could originate from within your App or in another App within the CUBID ecosystem.

#### Payload
```json
{
  "event": "blacklisted_stamp",
  "user_id": "123456",
  "stamp_type": "email",
  "is_login": true,
  "is_mandatory": true,
  "timestamp": "2024-03-14T12:34:56Z"
}
```

- `event`: The event type (`blacklisted_stamp`).
- `user_id`: The unique ID of the affected user.
- `stamp_type`: The type of the blacklisted stamp (e.g., `email`, `phone`).
- `is_login`: Whether the blacklisted stamp was used for logging into your App.
- `is_mandatory`: Whether the stamp is mandatory for your App.
- `timestamp`: When the event occurred.

#### Business Logic 
Recommendations
- **If `is_login = true`**: Lock the user’s account, notify them of the breach, guide them to correct the issue via CUBID, and then force a password reset when the stamp has been un-blacklisted.
- **If `is_login = false`**: You may choose not to act unless the stamp is critical to your business logic. Consider implementing a grace period to allow the user to correct the situation within CUBID.



## 2. Merged Users

**Trigger**: When a user merges multiple accounts to resolve issues like duplicate identities, resulting in one account being un-blacklisted and another account becoming obsolete, assuming both accounts were previously registered with your App.

#### Payload
```json
{
  "event": "merged_users",
  "user_id_primary": "primary_user_123",
  "user_id_obsolete": "obsolete_user_456",
  "timestamp": "2024-03-14T15:23:45Z"
}
```

- `user_id_primary`: The primary, un-blacklisted account.
- `user_id_obsolete`: The obsolete account that should be locked permanently.

#### Business Logic 
Recommendations
- Unlock the `user_id_primary` account and enforce a password reset, if applicable.
- Keep the `user_id_obsolete` account locked and deactivated.
- Consider allowing the user to port information from the `user_id_obsolete` to `user_id_primary` if that makes sense to your business logic.

#### Notes
- When a user takes action to un-blacklist their Stamps, it results in merged accounts. Typically, this will not affect your App though, since only one of their pre-merge accounts will likely be registered to your App. Such cases would NOT trigger this hook. (Though you would likely see the result as an increased score.)
- When both pre-merger accounts were registered within your scope, this hook will trigger. 



## 3. Score Increased

**Trigger**: When a user strengthens their identity by adding a new stamp or taking other actions that improve their overall score.

#### Payload
```json
{
  "event": "score_increased",
  "user_id": "user_789",
  "new_score": 85,
  "timestamp": "2024-03-15T10:00:00Z"
}
```

- `new_score`: The user’s updated score.

#### Business Logic 
Recommendations
- Congratulate the user when they log in, if applicable, and consider offering rewards for identity improvements.
- This trigger is broader in scope than the `new_stamp_added` hook, since it also captures information from stamps that the user may not have directly shared with your App. So in many cases you should consider taking action on the `new_stamp_added` events rather than this score increase. 
- Avoid raising concerns about oversharing by making it clear if the cause of the score increase is unknown to you. Examples:
  - "Your identity has just been strengthened! While we don’t have specific details, your recent actions in the CUBID ecosystem have enhanced your profile’s security and trust. Keep up the great work!"
  - "Great news! Your CUBID identity is now stronger. Though we don’t have the specifics, your recent efforts have made your profile even more secure and trustworthy."
  - "Your profile security just got a boost! While we don’t track the details, we noticed a CUBID Score increase. Well done!"
  - "Congrats! You've successfully strengthened your CUBID identity. We don’t have the exact details, but your recent steps have enhanced your security and trustworthiness."
  - "Your identity is now even more secure! Without knowing the specifics, we can tell that your CUBID profile has been further strengthened by your score increasing. Keep it up!"



## 4. Score Decreased

**Trigger**: When a user’s score drops due to a blacklisted or expired stamp. 

#### Payload
```json
{
  "event": "score_decreased",
  "user_id": "user_789",
  "new_score": 60,
  "timestamp": "2024-03-15T12:00:00Z"
}
```

- `new_score`: The user’s updated score after the decrease.

#### Business Logic 
Recommendations
- Take action if the user’s score drops below a threshold, especially if you offer gated content based on score.
- Consider implementing a grace period where the user can take corrective action to raise their score.
- Note: This trigger is broader in scope than the blacklisting hook, since it also captures information from stamps that the user may not have directly shared with your App. So in many cases you should consider taking action on the blacklisting events rather than this score decrease. 



## 5. New Stamp Added

**Trigger**: When a user adds a new stamp to their CUBID profile, strengthening their identity.

#### Payload
```json
{
  "event": "new_stamp_added",
  "user_id": "user_123",
  "stamp_type": "phone",
  "timestamp": "2024-03-16T08:30:00Z"
}
```

- `stamp_type`: The type of stamp added (e.g., `email`, `phone`).

#### Business Logic 
Recommendations
- Offer incentives or personalized greetings when users add important stamps (such as phone or email) that they’ve shared with your App.



## 6. Stamp Expired

**Trigger**: When a user’s stamp expires, leading to a potential drop in their identity strength.

#### Payload
```json
{
  "event": "stamp_expired",
  "user_id": "user_123",
  "stamp_type": "email",
  "timestamp": "2024-03-17T08:30:00Z"
}
```

- `stamp_type`: The expired stamp type (e.g., `email`, `phone`).

#### Business Logic 
Recommendations
- Notify users to revalidate or update expired stamps, and if applicable, temporarily restrict access until stamps are renewed.



## 7. Stamp Removed (Deauthorized)

**Trigger**: When a user revokes or deauthorizes a previously shared stamp for your App.

#### Payload
```json
{
  "event": "stamp_removed",
  "user_id": "user_123",
  "stamp_type": "email",
  "timestamp": "2024-03-17T08:30:00Z"
}
```

- `stamp_type`: The removed stamp type.

#### Business Logic 
Recommendations
- Adjust the user’s profile accordingly. If the removed stamp is critical (e.g., email), notify the user and ask them to provide a new stamp.



## 8. User Deactivated

**Trigger**: When a user’s CUBID account is deactivated or suspended.

#### Payload
```json
{
  "event": "user_deactivated",
  "user_id": "user_123",
  "timestamp": "2024-03-17T08:30:00Z"
}
```

#### Business Logic 
Recommendations
- Lock the user’s account in your App and prevent further access until the account is reactivated.
