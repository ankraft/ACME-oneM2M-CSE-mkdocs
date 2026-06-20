# Polling Channels

*2 test modules, 26 test cases. [&larr; Back to overview](index.md)*

??? note "`testPCH.py` — PollingChannel (&lt;PCH&gt;) resource functionality (13 tests)"

    | # | Test Method | Requests Performed |
    |---|---|---|
    | 1 | `test_createPCHwithWrongOriginatorFail` | CREATE a &lt;PCH&gt; under the &lt;AE&gt; using the CSE admin originator (not the AE's own originator) -> expects ORIGINATOR_HAS_NO_PRIVILEGE. |
    | 2 | `test_createPCH` | CREATE a &lt;PCH&gt; under the &lt;AE&gt; with the AE's own originator -> expects CREATED. |
    | 3 | `test_createSecondPCHFail` | CREATE a 2nd &lt;PCH&gt; under the same &lt;AE&gt; -> expects BAD_REQUEST (only one PCH allowed per originator/AE). |
    | 4 | `test_createPCHunderCSEBaseFail` | CREATE a &lt;PCH&gt; directly under the &lt;CSEBase&gt; -> expects INVALID_CHILD_RESOURCE_TYPE. |
    | 5 | `test_retrievePCH` | RETRIEVE the &lt;PCH&gt; -> expects OK. |
    | 6 | `test_retrievePCHwithWrongOriginatorFail` | RETRIEVE the &lt;PCH&gt; with an unauthorized originator -> expects ORIGINATOR_HAS_NO_PRIVILEGE. |
    | 7 | `test_retrievePCHWithAE2Fail` | RETRIEVE the &lt;PCH&gt; using a 2nd AE's originator (which has general ACP access to the 1st AE but not specifically to its PCH) -> expects ORIGINATOR_HAS_NO_PRIVILEGE. |
    | 8 | `test_attributesPCH` | RETRIEVE the &lt;PCH&gt; -> expects OK; checks `ty`, `ct`, `lt`, `et`, and that `ct` &lt;= `lt` &lt; `et`. |
    | 9 | `test_setAggreagstionState` | UPDATE the &lt;PCH&gt; setting `rqag=True` (request aggregation) -> expects UPDATED; checks the returned `rqag`. |
    | 10 | `test_getAggreagstionState` | RETRIEVE the &lt;PCH&gt; -> expects OK; checks `rqag` is `True`. |
    | 11 | `test_deletePCHwrongOriginatorFail` | DELETE the &lt;PCH&gt; with an unauthorized originator -> expects ORIGINATOR_HAS_NO_PRIVILEGE. |
    | 12 | `test_deletePCH` | DELETE the &lt;PCH&gt; with the correct originator -> expects DELETED. |
    | 13 | `test_retrievePCUAfterDeleteFail` | RETRIEVE the &lt;PCH&gt;'s `pcu` (pollingChannelURI) after the &lt;PCH&gt; was deleted -> expects NOT_FOUND. |

??? note "`testPCH_PCU.py` — PollingChannelURI (&lt;PCU&gt;) functionality (13 tests)"

    | # | Test Method | Requests Performed |
    |---|---|---|
    | 1 | `test_createSUBunderCNTFail` | CREATE a &lt;SUB&gt; under &lt;CNT&gt; targeting the 2nd AE for notifications, before any &lt;PCH&gt; exists for that AE -> expects SUBSCRIPTION_VERIFICATION_INITIATION_FAILED (no channel to deliver the verification request). |
    | 2 | `test_createPCHunderAE2` | CREATE a &lt;PCH&gt; under the 2nd &lt;AE&gt; with `rqag=False` -> expects CREATED. |
    | 3 | `test_retrievePCUunderAE2Fail` | RETRIEVE the 2nd AE's `pcu` with nothing pending -> expects REQUEST_TIMEOUT (implicit timeout waiting for a request to poll). |
    | 4 | `test_createSUBunderCNT` | In parallel: start a background thread polling the 2nd AE's `pcu` and respond OK to the incoming verification request, while the main thread CREATEs a &lt;SUB&gt; under &lt;CNT&gt; targeting the 2nd AE -> CREATE expects CREATED; the polling thread's verification handling is checked internally; waits for the polling thread to finish. |
    | 5 | `test_DeleteSUBunderCNT` | In parallel: poll the 2nd AE's `pcu` expecting a subscription-deletion notification (`sud`) while DELETEing the &lt;SUB&gt; -> DELETE expects DELETED; polling thread validates and answers the delete notification. |
    | 6 | `test_createSUB2underCNTAnswerWithWrongTargetFail` | Poll using the 2nd AE's originator (expecting a REQUEST_TIMEOUT since no request will arrive there) while CREATEing a &lt;SUB&gt; with the *wrong* originator (the 2nd AE itself, lacking permission) -> CREATE expects ORIGINATOR_HAS_NO_PRIVILEGE; no subscription created. |
    | 7 | `test_createSUB2underCNTAnswerWithEmptyAnswerFail` | Poll and answer the verification request with an empty (`{}`) notification body, while CREATEing a &lt;SUB&gt; -> CREATE expects SUBSCRIPTION_VERIFICATION_INITIATION_FAILED (malformed verification response). |
    | 8 | `test_createSUB2underCNTAnswerWithWrongAnswerFail` | Poll and answer the verification request with a wrongly-typed notification body (`m2m:rqp` instead of `m2m:rsp`), while CREATEing a &lt;SUB&gt; -> CREATE expects SUBSCRIPTION_VERIFICATION_INITIATION_FAILED. |
    | 9 | `test_accesPCUwithWrongOriginator` | Poll the 2nd AE's `pcu` using the 1st AE's originator (which lacks polling rights there) -> expects ORIGINATOR_HAS_NO_PRIVILEGE. |
    | 10 | `test_accessPCUwithshortExpiration` | RETRIEVE the 2nd AE's `pcu` with a short request-expiration header (half the normal delay) and nothing pending -> expects REQUEST_TIMEOUT. |
    | 11 | `test_updatePCHaggregate` | UPDATE the 2nd AE's &lt;PCH&gt; setting `rqag=True` -> expects UPDATED; checks the returned `rqag`. |
    | 12 | `test_aggregation` | Poll and answer a &lt;SUB&gt; verification request while CREATEing a &lt;SUB&gt; under &lt;CNT&gt; -> expects CREATED; UPDATE the &lt;PCH&gt; to enable aggregation (`rqag=True`) -> expects UPDATED; CREATE 5 &lt;CIN&gt;s concurrently in separate threads (each triggers a notification) -> each expects CREATED; poll the `pcu` once more with `aggregated=True` -> expects OK, and the response is validated as an `m2m:agrp` list containing all the aggregated notification requests, each individually answered OK. |
    | 13 | `test_createNotificationDoPolling` | CREATE a &lt;CIN&gt; under &lt;CNT&gt; (intended to trigger a notification for later polling, marked TODO/incomplete in the source) -> expects CREATED. |
