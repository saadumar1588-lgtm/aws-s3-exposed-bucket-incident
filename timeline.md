# Incident Timeline

All times are shown in both PKT (UTC+05:00) and UTC. PKT = UTC + 5 hours.
The source IP of all reads was the same and is hidden for privacy (`<MY_IP>`).

| PKT (15 Oct = next day) | UTC | Event | Source |
|---|---|---|---|
| 14 Oct 18:52:31 | 13:52:31 | Bucket created (`CreateBucket`) | CloudTrail |
| 14 Oct 18:52:32 | 13:52:32 | Default encryption set (`PutBucketEncryption`) | CloudTrail |
| 14 Oct 18:53:55 | 13:53:55 | Config first recorded the bucket | Config |
| 14 Oct 19:34:21 | 14:34:21 | Block Public Access setting saved (`PutBucketPublicAccessBlock`); all four values were already `false` in Config before and after | CloudTrail, Config |
| 14 Oct 19:34:48 | 14:34:48 | **Public-read bucket policy added (`PutBucketPolicy`). Exposure starts.** | CloudTrail |
| 14 Oct 19:36:17 | 14:36:17 | Config captured the policy change (`policyText` went from `null` to the public policy) | Config |
| 14 Oct 19:39:23 | 14:39:23 | Read 1: `GetObject customers.csv`, Windows Chrome, success | CloudTrail data events |
| 14 Oct 19:41:20 | 14:41:20 | Read 2: `GetObject customers.csv`, WhatsApp user agent (link fetch), success | CloudTrail data events |
| 14 Oct 19:41:28 | 14:41:28 | Read 3: `GetObject customers.csv`, Android Chrome, success | CloudTrail data events |
| 15 Oct 00:39:28 | 19:39:28 (14 Oct) | **Bucket policy deleted (`DeleteBucketPolicy`). Exposure ends.** | CloudTrail |
| 15 Oct 00:40:11 | 19:40:11 (14 Oct) | Block Public Access turned on, all four settings `true` | CloudTrail |
| 15 Oct 00:41:06 | 19:41:06 (14 Oct) | Verification read: `AccessDenied` | CloudTrail data events |
| 15 Oct 00:41:07 | 19:41:07 (14 Oct) | Verification read: `AccessDenied` | CloudTrail data events |
| 15 Oct 01:15:46 | 20:15:46 (14 Oct) | Bucket deleted (`DeleteBucket`) | CloudTrail |

## Key numbers

- Exposure window: 19:34:48 to 00:39:28 PKT = 5 h 4 min 40 s
- Successful reads during exposure: 3, all within the first 7 minutes
- Reads blocked after containment: 2 (the analyst's own verification)

## Limitations

- Detection time: the Config rule showed the bucket as noncompliant, but the exact time was not recorded. [Not Recorded].
- The Config timeline showed no compliance history and a "stopped recording changes to compliance events" entry, so the detection delay could not be measured from Config.
- Reads served from the browser cache would not appear in CloudTrail, so the count of 3 is a lower bound.
