# Core CSE & Registration

*5 test modules, 81 test cases. [&larr; Back to overview](index.md)*

??? note "`testCSE.py` — CSEBase (&lt;CSE&gt;) resource functionality (8 tests)"

    | # | Test Method | Requests Performed |
    |---|---|---|
    | 1 | `test_retrieveCSE` | RETRIEVE the &lt;CSEBase&gt; with a valid originator -> expects OK. |
    | 2 | `test_retrieveCSEWithWrongOriginator` | RETRIEVE the &lt;CSEBase&gt; with an invalid/unauthorized originator (`CWrong`) -> expects ORIGINATOR_HAS_NO_PRIVILEGE. |
    | 3 | `test_attributesCSE` | RETRIEVE the &lt;CSEBase&gt; -> expects OK; validates resource attributes in the response (`csi`, `pi`, `rn`, `ty`, `ri`, `ct`, `cst`, `srt`, `srv`) against expected values/types. |
    | 4 | `test_CSEreleaseVersion` | RETRIEVE the &lt;CSEBase&gt; -> expects OK; checks the `X-M2M-RVI` response header value is present in the resource's `srv` (supportedReleaseVersions) attribute. |
    | 5 | `test_attributeCSEctm` | RETRIEVE the &lt;CSEBase&gt; -> expects OK; validates the `ctm` (current time) attribute is a well-formed ISO 8601 timestamp. |
    | 6 | `test_CSESupportedResourceTypes` | RETRIEVE the &lt;CSEBase&gt; -> expects OK; checks that the `srt` (supportedResourceTypes) attribute lists every resource type the CSE supports. |
    | 7 | `test_deleteCSEFail` | DELETE the &lt;CSEBase&gt; -> expects OPERATION_NOT_ALLOWED. |
    | 8 | `test_updateCSEFail` | UPDATE the &lt;CSEBase&gt; setting `lbl` -> expects OPERATION_NOT_ALLOWED. |

??? note "`testAE.py` — Application Entity (&lt;AE&gt;) registration, update, deletion, and attribute handling (27 tests)"

    | # | Test Method | Requests Performed |
    |---|---|---|
    | 1 | `test_createAE` | CREATE/register an &lt;AE&gt; under &lt;CSEBase&gt; using the self-registration originator -> expects CREATED; stores the assigned `aei` and `acpi`. |
    | 2 | `test_createAEUnderAEFail` | CREATE an &lt;AE&gt; under another &lt;AE&gt; (invalid parent) -> expects INVALID_CHILD_RESOURCE_TYPE. |
    | 3 | `test_createAEAgainFail` | CREATE an &lt;AE&gt; reusing the same `rn` as an existing registration -> expects CONFLICT. |
    | 4 | `test_createAEWithExistingOriginatorFail` | CREATE an &lt;AE&gt; (no `rn`) using an originator that's already registered -> expects ORIGINATOR_HAS_ALREADY_REGISTERED. |
    | 5 | `test_createAECSIoriginatorFail` | CREATE an &lt;AE&gt; using the CSE-ID itself as originator -> expects SECURITY_ASSOCIATION_REQUIRED. |
    | 6 | `test_retrieveAE` | RETRIEVE the &lt;AE&gt; -> expects OK. |
    | 7 | `test_retrieveAEWithWrongOriginator` | RETRIEVE the &lt;AE&gt; with an unauthorized originator -> expects one of ORIGINATOR_HAS_NO_PRIVILEGE / SERVICE_SUBSCRIPTION_NOT_ESTABLISHED. |
    | 8 | `test_attributesAE` | RETRIEVE the &lt;AE&gt; -> expects OK; checks `aei`, `ty`, `api`, `ct`/`lt`/`et` ordering, `rr=False`, `srv`, `st` (absent), `pi` matches CSEBase's `ri`. |
    | 9 | `test_updateAELbl` | UPDATE the &lt;AE&gt; setting `lbl` -> expects UPDATED; RETRIEVE it again -> expects OK, checks `lbl` contains the new tag. |
    | 10 | `test_updateAETy` | UPDATE the &lt;AE&gt; attempting to change `ty` to CSEBase -> expects BAD_REQUEST. |
    | 11 | `test_updateAEPi` | UPDATE the &lt;AE&gt; attempting to change `pi` -> expects BAD_REQUEST. |
    | 12 | `test_updateAEUnknownAttribute` | UPDATE the &lt;AE&gt; with an unknown attribute -> expects BAD_REQUEST. |
    | 13 | `test_deleteAEByUnknownOriginator` | DELETE the &lt;AE&gt; with an unauthorized originator -> expects ORIGINATOR_HAS_NO_PRIVILEGE. |
    | 14 | `test_deleteAEByAssignedOriginator` | DELETE the &lt;AE&gt; with the correct originator -> expects DELETED. |
    | 15 | `test_createAEWrongCSZ` | CREATE an &lt;AE&gt; with an invalid `csz` (content serialization) value -> expects BAD_REQUEST. |
    | 16 | `test_createAECSZ` | CREATE an &lt;AE&gt; with valid `csz` values (`application/cbor`, `application/json`) -> expects CREATED. |
    | 17 | `test_deleteAECSZ` | DELETE the CSZ-configured &lt;AE&gt; -> expects DELETED. |
    | 18 | `test_createAENoAPI` | CREATE an &lt;AE&gt; missing the mandatory `api` attribute -> expects something other than CREATED. |
    | 19 | `test_createAEAPIWrongPrefix` | CREATE an &lt;AE&gt; with an `api` value using an unknown prefix character (`X`) -> expects something other than CREATED. |
    | 20 | `test_createAEAPICorrectR` | CREATE an &lt;AE&gt; with a valid `api` using the 'Registered' (`R`) prefix -> expects CREATED; DELETE it -> expects DELETED. |
    | 21 | `test_createAEAPICorrectN` | CREATE an &lt;AE&gt; with a valid `api` using the 'Non-Registered' (`N`) prefix -> expects CREATED; DELETE it -> expects DELETED. |
    | 22 | `test_createAEAPIRVI3LowerCaseR` | CREATE an &lt;AE&gt; with a lower-case `api` prefix under release version 3 (`X-M2M-RVI: 3`) -> expects CREATED (lowercase allowed pre-RVI4); DELETE it -> expects DELETED. |
    | 23 | `test_createAEAPIRVI4LowerCaseRFail` | Same lower-case `api` prefix but under release version 4 -> expects BAD_REQUEST (lowercase no longer allowed). |
    | 24 | `test_createAENoOriginator` | CREATE an &lt;AE&gt; with no originator at all -> expects CREATED (CSE auto-assigns one); checks `aei` is present; DELETE it -> expects DELETED. |
    | 25 | `test_createAEEmptyOriginator` | CREATE an &lt;AE&gt; with an explicitly empty-string originator -> expects CREATED; checks `aei`; DELETE it -> expects DELETED. |
    | 26 | `test_createAEInvalidRNFail` | CREATE an &lt;AE&gt; with an `rn` containing a disallowed character (`?`) -> expects BAD_REQUEST; CREATE another with a space in `rn` -> expects BAD_REQUEST. |
    | 27 | `test_createAEWithCreatorFail` | CREATE an &lt;AE&gt; with `cr` explicitly set to `None` -> expects BAD_REQUEST (creator attribute not permitted for AE self-registration). |

??? note "`testNOD.py` — Node (&lt;NOD&gt;) resource lifecycle and notifications (13 tests)"

    | # | Test Method | Requests Performed |
    |---|---|---|
    | 1 | `test_createNOD` | CREATE a &lt;NOD&gt; under the &lt;CSEBase&gt; with a node-ID (`ni`) -> expects CREATED; checks `ri` is present. |
    | 2 | `test_retrieveNOD` | RETRIEVE the &lt;NOD&gt; -> expects OK. |
    | 3 | `test_retrieveNODWithWrongOriginator` | RETRIEVE the &lt;NOD&gt; with an unauthorized originator -> expects ORIGINATOR_HAS_NO_PRIVILEGE. |
    | 4 | `test_attributesNOD` | RETRIEVE the &lt;NOD&gt; -> expects OK; checks `ty`, `pi` (matches the CSEBase's `ri`), `rn`, `ct`, `lt`, `et`, and `ni`. |
    | 5 | `test_updateNODLbl` | UPDATE the &lt;NOD&gt; setting `lbl` -> expects UPDATED; RETRIEVE it again -> expects OK, checks `lbl` contains the new tag. |
    | 6 | `test_updateNODUnknownAttribute` | UPDATE the &lt;NOD&gt; with an unknown attribute -> expects BAD_REQUEST. |
    | 7 | `test_createAEForNOD` | CREATE an &lt;AE&gt; with its `nl` (node link) pointing to the &lt;NOD&gt; -> expects CREATED, checks `nl`/`ri`/`aei`; RETRIEVE the &lt;NOD&gt; -> expects OK, checks the AE's `ri` is listed in the node's `hael` (hosted AE list). |
    | 8 | `test_deleteAEForNOD` | DELETE the &lt;AE&gt; -> expects DELETED; RETRIEVE the &lt;NOD&gt; -> expects OK, checks `hael` is now absent (no more hosted AEs). |
    | 9 | `test_moveAEToNOD2` | Re-create the &lt;AE&gt; linked to the first &lt;NOD&gt; (calls test_createAEForNOD); CREATE a 2nd &lt;NOD&gt; -> expects CREATED; UPDATE the &lt;AE&gt; to set `nl` to the 2nd node -> expects UPDATED, checks `nl`; RETRIEVE the 1st &lt;NOD&gt; -> expects OK, checks `hael` is now absent; RETRIEVE the 2nd &lt;NOD&gt; -> expects OK, checks `hael` now contains the AE's `ri`. |
    | 10 | `test_deleteNOD2` | DELETE the 2nd &lt;NOD&gt; -> expects DELETED; RETRIEVE the &lt;AE&gt; -> expects OK, checks `nl` is now absent (link removed when its target node was deleted). |
    | 11 | `test_deleteNOD` | DELETE the (first) &lt;NOD&gt; -> expects DELETED. |
    | 12 | `test_createNODEmptyHael` | CREATE a &lt;NOD&gt; with an explicit empty `hael` list -> expects BAD_REQUEST. |
    | 13 | `test_createNODDoubleHael` | CREATE a &lt;NOD&gt; with a pre-set `hael` list containing one entry -> expects CREATED, checks `hael` has length 1; CREATE an &lt;AE&gt; whose `nl` points to that node -> expects CREATED; RETRIEVE the &lt;NOD&gt; -> expects OK, checks `hael` still has exactly 1 entry containing the AE's `ri` (no duplicate); DELETE the &lt;AE&gt; and the &lt;NOD&gt; -> each expects DELETED. |

??? note "`testRequests.py` — General request handling, headers, and response codes (17 tests)"

    | # | Test Method | Requests Performed |
    |---|---|---|
    | 1 | `test_OETnow` | RETRIEVE the &lt;AE&gt; with the `OET` (Operation Execution Time) header set to the current time -> expects OK. |
    | 2 | `test_OETpast` | RETRIEVE the &lt;AE&gt; with `OET` set 10 seconds in the past -> expects OK. |
    | 3 | `test_OETfuture` | RETRIEVE the &lt;AE&gt; with `OET` set to an absolute future timestamp -> expects OK; checks the request actually waited until that time before executing. |
    | 4 | `test_OETfuturePeriod` | RETRIEVE the &lt;AE&gt; with `OET` given as an ISO-8601 period (e.g. `PT2S`) in the future -> expects OK; checks the request waited the corresponding time. |
    | 5 | `test_OETfutureSeconds` | RETRIEVE the &lt;AE&gt; with `OET` given in milliseconds-from-now -> expects OK; checks the request waited accordingly. |
    | 6 | `test_OETafterRSETFail` | RETRIEVE the &lt;AE&gt; with `OET` set later than `RSET` (Result Set Expiration Time) -> expects BAD_REQUEST. |
    | 7 | `test_OETafterRQETFail` | RETRIEVE the &lt;AE&gt; with `OET` set later than `RET`/`RQET` (Request Expiration Time) -> expects BAD_REQUEST. |
    | 8 | `test_RETnowFail` | RETRIEVE the &lt;AE&gt; with `RET` set to right now -> expects REQUEST_TIMEOUT (no time left to process). |
    | 9 | `test_RETpastFail` | RETRIEVE the &lt;AE&gt; with `RET` set 10 seconds in the past -> expects REQUEST_TIMEOUT. |
    | 10 | `test_RETfuture` | RETRIEVE the &lt;AE&gt; with `RET` set to an absolute future timestamp -> expects OK. |
    | 11 | `test_RETpastSecondsFail` | RETRIEVE the &lt;AE&gt; with `RET` given as a negative milliseconds offset -> expects REQUEST_TIMEOUT. |
    | 12 | `test_RETfutureSeconds` | RETRIEVE the &lt;AE&gt; with `RET` given in milliseconds-from-now (positive) -> expects OK. |
    | 13 | `test_OETRETfutureSeconds` | RETRIEVE the &lt;AE&gt; with both `RET` and `OET` in the future, `OET` earlier than `RET` -> expects OK (valid ordering). |
    | 14 | `test_OETRETfutureSecondsWrongFail` | RETRIEVE the &lt;AE&gt; with `OET` later than `RET` (invalid ordering) -> expects BAD_REQUEST. |
    | 15 | `test_RSETsmallerThanRETFail` | RETRIEVE the &lt;AE&gt; with `RST` (Result Set Time?) smaller than `RET` in an invalid combination -> expects BAD_REQUEST. |
    | 16 | `test_RSETpastFail` | RETRIEVE the &lt;AE&gt; with `RST` set in the past -> expects REQUEST_TIMEOUT. |
    | 17 | `test_RSETNonBlockingSynchFail` | RETRIEVE the &lt;AE&gt; with `rt=nonBlockingRequestSynch` and a short `RST` header -> expects ACCEPTED_NON_BLOCKING_REQUEST_SYNC, checks the `m2m:uri` reference and the echoed `RST` header; sleeps past the request-expiration window; RETRIEVE the referenced result resource -> expects NOT_FOUND (result expired before being collected). |

??? note "`testAddressing.py` — CSE resource addressing methods (structured/unstructured, CSE-relative, SP-relative, etc.) (16 tests)"

    | # | Test Method | Requests Performed |
    |---|---|---|
    | 1 | `test_cseRelativeStructured` | RETRIEVE a &lt;CNT&gt; via CSE-relative structured addressing (`{cseRN}/{aeRN}/{cntRN}`) -> expects OK; repeats using the `-` CSEBase placeholder in place of the CSE name -> expects OK; both check the returned `rn`. |
    | 2 | `test_cseRelativeStructuredPlaceholder` | RETRIEVE the &lt;CNT&gt; via CSE-relative structured addressing using the `-` placeholder for the CSEBase name -> expects OK; checks `rn`. |
    | 3 | `test_cseRelativeUnstructured` | RETRIEVE the &lt;CNT&gt; via CSE-relative unstructured addressing (CSE root + resource-ID) -> expects OK; checks `rn`. |
    | 4 | `test_spRelativeStructured` | RETRIEVE the &lt;CNT&gt; via SP-relative structured addressing (`/{cseId}/{cseRN}/{aeRN}/{cntRN}`) -> expects OK; repeats with the `-` CSEBase placeholder -> expects OK; both check `rn`. |
    | 5 | `test_spRelativeStructuredPlaceholder` | RETRIEVE the &lt;CNT&gt; via SP-relative structured addressing using the `-` CSEBase placeholder -> expects OK; checks `rn`. |
    | 6 | `test_spRelativeUnstructured` | RETRIEVE the &lt;CNT&gt; via SP-relative unstructured addressing (`/{cseId}/{resourceId}`) -> expects OK; checks `rn`. |
    | 7 | `test_spRelativeCSEIDFail` | RETRIEVE using SP-relative addressing pointing only at the bare CSE-ID (no resource path) -> expects BAD_REQUEST. |
    | 8 | `test_absoluteStructured` | RETRIEVE the &lt;CNT&gt; via absolute structured addressing (`//{spId}{cseId}/{cseRN}/{aeRN}/{cntRN}`) -> expects OK; repeats with the `-` placeholder -> expects OK; both check `rn`. |
    | 9 | `test_absoluteStructuredPlaceholder` | RETRIEVE the &lt;CNT&gt; via absolute structured addressing using the `-` CSEBase placeholder -> expects OK. |
    | 10 | `test_absoluteUnstructured` | RETRIEVE the &lt;CNT&gt; via absolute unstructured addressing (`//{spId}{cseId}/{resourceId}`) -> expects OK; checks `rn`. |
    | 11 | `test_absoluteStructuredWrongSPIDFail` | RETRIEVE via absolute structured addressing with an incorrect/unknown SP-ID -> expects NOT_FOUND. |
    | 12 | `test_absoluteUnstructuredWrongSPIDFail` | RETRIEVE via absolute unstructured addressing with an incorrect/unknown SP-ID -> expects NOT_FOUND. |
    | 13 | `test_absoluteCSEIDFail` | RETRIEVE via absolute addressing pointing only at the bare SP-ID + CSE-ID (no resource path) -> expects BAD_REQUEST. |
    | 14 | `test_cseRelativeTrailingSlashFail` | RETRIEVE using CSE-relative addressing with a trailing slash after the CSE resource name -> expects BAD_REQUEST. |
    | 15 | `test_spRelativeTrailingSlashFail` | RETRIEVE using SP-relative addressing with a trailing slash -> expects BAD_REQUEST. |
    | 16 | `test_absoluteTrailingSlashFail` | RETRIEVE using absolute addressing with a trailing slash -> expects BAD_REQUEST. |
