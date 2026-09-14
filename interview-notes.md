# Troubleshooting Drill & Interview Prep

## Troubleshooting Drill — Missing DeleteBucket Event

**Scenario:** A bucket disappeared, but `DeleteBucket` can't be found in CloudTrail.

**Investigation checklist:**

1. Confirm the correct Region is selected (the bucket's Region, not just "usual").
2. Expand the time range — event delivery can take several minutes.
3. Clear existing filters, then search the exact event name `DeleteBucket`.
4. Verify you're signed into the correct AWS account.
5. Confirm your identity has permission to view CloudTrail history.
6. Check for actions by assumed roles or AWS services, not just your own username.
7. Confirm this is a management event — Event History does not show S3 object-level data events.

**Most likely causes:** wrong Region, wrong account, active filter, event-delivery delay, or searching under the wrong identity.

> CloudTrail Event History is regional and shows management events only, not every possible event type.

