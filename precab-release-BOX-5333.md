# BOX-5333: Allow Duplicate License Numbers for Different States (NA)

## Executive Summary

This release enables North American customers to add drivers with the same license number as long as the issuing state is different. Currently, MyEROAD prevents duplicate license numbers entirely, which blocks legitimate use cases where drivers from different states may have the same ID number. This change allows duplicate license numbers while maintaining uniqueness by state within each organization, supporting large enterprise customers with multi-state operations.

## Environments

- MyEROAD NA

## Release Details

- **Product:** MyEROAD Driver Service
- **Type of Release:** Backend change
- **Release Contact:** Gwener Mitchell Gutierrez / Rizwan Ahmad
- **Date:** Right after CAB approval
- **Risk:** Medium
- **Systems Impacted:** MyEROAD Driver Service

## Release Steps

1. Box Office Team merges gate PR
2. Deploy Driver Service

## Rollback Plan

- Use Concourse to rollback to previous version
- Revert code commits if needed

## Tickets

- CAB Request: BOX-5333
- GitHub PR: https://github.com/eroad/myeroad-driver-service/pull/XXX

## Business Context

Confirmed with David Blackwell that large enterprise customers need to support duplicate license numbers across different states. In the US, driver's license numbers are not unique across states - each state has its own numbering system. A driver's license is only unique within a state. This change aligns MyEROAD with real-world licensing requirements in North America.

## Change Summary

- Backend now allows duplicate license numbers if the issuing state differs
- Uniqueness validation changed from license number alone to license number + state combination
- Only applies to North America region
- Alias field remains unique as before
- New Zealand continues to enforce unique license numbers (no change)

## Important Notes

**Important:** Testing completed on test, apacpp, and napp environments with no regressions. However, there is a driver ID uniqueness issue: driver ID is derived from the license number, which now throws an error saying driver ID is not unique when creating or updating drivers with duplicate license numbers across states.

## Quality Metrics

- Unit tests completed
- Integration tests passed on test environments
- Manual QA completed
- Known issue: Driver ID uniqueness validation needs follow-up work
