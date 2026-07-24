# Design Document -- Issue #201: Outage map displaying incorrectly
Generated: 2026-07-24 | Artifact: `issue-201-outage-map-displaying-incorrectly-design.md`

## Executive Summary
**Issue**: Outage map displaying incorrectly
**Description**: Crew locations on the outage map are wrong. Field view shows incorrect crew positions. SAP_ADMS_UpdateCrewStatus not reflecting real-time data.
**Branch**: work/issue-201-bob-sdlc

## Root Cause
Outage event logging is inconsistent because the logging path is not reached under all flow branches, resulting in silent failures and missing audit records.

## Approach
Audit all terminal nodes in the outage flow. Add LogOutageEvent() helper in SAP_ADMS_CommonFunctions.esql. Insert the logging call on every non-error exit path so all outage events produce an audit record.

## Files to Change
- SAP_ADMS_UpdateCrewStatus_Compute.esql
- SAP_ADMS_Config.properties

## Functions to Add
- LogOutageEvent(incidentId, eventType, status, details) -- Writes a structured outage audit record

## Acceptance Criteria
- Fix resolves the reported behaviour: Outage map displaying incorrectly
- No hardcoded values in ESQL -- all configurable (CR-011 PASS)
- Code coverage >= 80% (QR-041 PASS)
- Performance overhead < 5ms per call (DR-014 PASS)
- All existing unit tests continue to pass
- Error handling present on all new code paths (CR-009 PASS)

## Design Rules Applied
- DR-001: Naming convention PASS
- DR-005: Utility pattern PASS
- DR-009: REST API standards PASS
- DR-011: Security -- no credentials in code PASS
- DR-014: Performance -- less than 5ms overhead PASS

## Status: Design Complete
