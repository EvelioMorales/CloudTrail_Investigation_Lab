# Findings — CreateBucket Event

## Summary View (CloudTrail Event History)

| Field | Value |
|---|---|
| Event name | CreateBucket |
| Event time | September 12, 2026, 16:41:08 (UTC-05:00) |
| Event source | s3.amazonaws.com |
| User name | root |
| AWS access key | ASIA4MI2KBUM7XH2IUSC |
| Source IP address | 76.31.133.208 |
| Event ID | f084ba5e-2933-45b2-8c7b-36304de2606c |
| Request ID | VS6H12S1DCJ72YR4 |

## Raw Event (selected fields)

```json
{
  "eventVersion": "1.11",
  "userIdentity": {
    "type": "Root",
    "principalId": "850995580185",
    "arn": "arn:aws:iam::850995580185:root",
    "accountId": "850995580185",
    "accessKeyId": "ASIA4MI2KBUM7XH2IUSC",
    "sessionContext": {
      "attributes": {
        "creationDate": "2026-09-12T21:26:27Z",
        "mfaAuthenticated": "false"
      }
    }
  },
  "eventTime": "2026-09-12T21:41:08Z",
  "eventSource": "s3.amazonaws.com",
  "eventName": "CreateBucket",
  "awsRegion": "us-east-2",
  "sourceIPAddress": "76.31.133.208",
  "requestParameters": {
    "CreateBucketConfiguration": {
      "LocationConstraint": "us-east-2"
    },
    "bucketName": "em-cloudtrail-investlab"
  },
  "responseElements": null,
  "requestID": "VS6H12S1DCJ72YR4",
  "eventID": "f084ba5e-2933-45b2-8c7b-36304de2606c",
  "readOnly": false,
  "eventType": "AwsApiCall",
  "managementEvent": true,
  "recipientAccountId": "850995580185",
  "eventCategory": "Management"
}
```

## Analysis

- **Identity:** The action was performed by the account `root` user (not an IAM user or assumed role), account ID `850995580185`, using a temporary session (`accessKeyId` begins with `ASIA`, indicating STS-issued credentials rather than long-term root keys).
- **MFA:** `mfaAuthenticated` is `false` for this session — worth flagging as a hardening item (root should always use MFA, and ideally root shouldn't be used for routine actions like bucket creation at all).
- **Source IP:** `76.31.133.208`, consistent with the operator's own network at the time of the action.
- **Timing:** The bucket was created at `2026-09-12T21:41:08Z` (16:41:08 CT), just ~15 minutes after the session began (`creationDate: 2026-09-12T21:26:27Z`).
- **Action scope:** `readOnly: false` and `eventCategory: Management` confirm this is a write/management-plane action, which is what CloudTrail Event History captures by default (S3 object-level data events require a separate data event trail).
- **Conclusion:** Identity, source IP, and timestamp all match expected activity for this lab — the event is verified as authorized.

## Related Event

- `PutBucketEncryption` was logged immediately after `CreateBucket` (same timestamp, `s3.amazonaws.com`), indicating default encryption was applied to the bucket right after creation.
