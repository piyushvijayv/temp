# Design Document -- Issue #61
Generated: 2026-07-21

## Executive Summary
**Issue**: outage is not getting logged correclty
**Description**: outage is not getting logged correclty
**Branch**: work/issue-61-bob-sdlc

## Architecture Decision
- Root cause: timestamps stored as UTC integers without timezone metadata
- Fix: add 5 ESQL timezone functions to SAP_ADMS_CommonFunctions.esql
- Pattern: Utility Function Library (Design Rule DR-005)

## Functions Designed
| Function | Purpose |
|---|---|
| GetTimezoneOffset(opco) | Returns UTC offset for OpCo |
| ConvertUTCtoLocal(ts, opco) | Converts UTC to local time |
| FormatETRDisplay(ts, opco) | Formats ETR for SAP mobile |
| ValidateTimezone(tz) | Validates timezone string |
| GetCurrentLocalTime(opco) | Returns current local time |

## Data Flow
SAP Mobile App -> ACE -> SAP_ADMS_UpdateIncidentETR_Compute.esql
                           -> ConvertUTCtoLocal(ETR_UTC, OpCoId)
                        Local ETR -> SAP Response -> Mobile Display

## Design Rules Applied
- DR-001: Naming convention PASS
- DR-005: Utility pattern PASS
- DR-009: REST API standards PASS
- DR-011: Security -- no credentials in code PASS
- DR-014: Performance -- less than 5ms overhead PASS

## Status: Design Complete
