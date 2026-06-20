# Load & Miscellaneous

*3 test modules, 51 test cases. [&larr; Back to overview](index.md)*

??? note "`testLoad.py` — Load/stress testing of the CSE (12 tests)"

    | # | Test Method | Requests Performed |
    |---|---|---|
    | 1 | `test_createAEs` | CREATE `count` &lt;AE&gt;s sequentially (count parameterized as 10/100/1000 by the test runner) -> each expects CREATED; measures and prints elapsed time. |
    | 2 | `test_retrieveAEs` | RETRIEVE each of the previously created &lt;AE&gt;s sequentially -> each expects OK, checks the returned `ri` matches; measures elapsed time. |
    | 3 | `test_deleteAEs` | DELETE each of the previously created &lt;AE&gt;s sequentially -> each expects DELETED; measures elapsed time. |
    | 4 | `test_createAEsParallel` | CREATE `count` &lt;AE&gt;s per thread across `parallel` concurrent threads -> each expects CREATED; measures elapsed time for the whole parallel batch. |
    | 5 | `test_deleteAEsParallel` | DELETE the previously created &lt;AE&gt;s, split evenly across `parallel` concurrent threads -> each expects DELETED; measures elapsed time. |
    | 6 | `test_createCNTCINs` | CREATE 1 &lt;AE&gt; -> expects CREATED; CREATE `count` &lt;CNT&gt;s under it (each with `mni=10`) -> each expects CREATED; CREATE 20 &lt;CIN&gt;s under each &lt;CNT&gt; -> each expects CREATED; measures elapsed time per CIN. |
    | 7 | `test_createCNTCINsParallel` | Same as `test_createCNTCINs` (1 &lt;AE&gt;, `count` &lt;CNT&gt;s, 20 &lt;CIN&gt;s each) but the per-&lt;CNT&gt; CIN creation runs in parallel threads (one thread per &lt;CNT&gt;) -> each CREATE expects CREATED; measures elapsed time. |
    | 8 | `test_deleteCNTCINs` | DELETE the single &lt;AE&gt; created by the CNT/CIN load tests (cascading delete of all child &lt;CNT&gt;/&lt;CIN&gt;s) -> expects DELETED; measures elapsed time. |
    | 9 | `test_storeImages` | CREATE 1 &lt;AE&gt; and 1 &lt;CNT&gt; (with `mni` sized to the count) -> each expects CREATED; CREATE `count` &lt;CIN&gt;s, each with a real JPEG image (base64) as `con` -> each expects CREATED; DELETE the &lt;AE&gt;; measures elapsed time. |
    | 10 | `test_storeImagesNoResponse` | Same as `test_storeImages` but each &lt;CIN&gt; CREATE request uses `rt=nonBlockingRequestSynch`/`noResponse` response type -> each expects NO_CONTENT instead of CREATED; DELETE the &lt;AE&gt;; measures elapsed time. |
    | 11 | `test_storeSimpleCIN` | CREATE 1 &lt;AE&gt; and 1 &lt;CNT&gt; -> each expects CREATED; CREATE `count` &lt;CIN&gt;s with a small numeric string `con` ('23.5') -> each expects CREATED; DELETE the &lt;AE&gt;; measures elapsed time. |
    | 12 | `test_storeSimpleCINNoResponse` | Same as `test_storeSimpleCIN` but using the "no response" response type for each CREATE -> each expects NO_CONTENT; DELETE the &lt;AE&gt;; measures elapsed time. |

??? note "`testMisc.py` — Miscellaneous CSE functionality not covered by a dedicated resource-type test file (37 tests)"

    | # | Test Method | Requests Performed |
    |---|---|---|
    | 1 | `test_checkHTTPRVI` | RETRIEVE the &lt;CSEBase&gt; -> expects OK; checks the `X-M2M-RVI` response header matches the configured release version. |
    | 2 | `test_checkHTTPRET` | RETRIEVE the &lt;CSEBase&gt; with an absolute `RET` header 10s in the future -> expects OK. |
    | 3 | `test_checkHTTPVSI` | RETRIEVE the &lt;CSEBase&gt; with a `VSI` (vendor info) header -> expects OK; checks the header is echoed back. |
    | 4 | `test_checkHTTPRETRelative` | RETRIEVE the &lt;CSEBase&gt; with a relative (milliseconds) `RET` header -> expects OK. |
    | 5 | `test_checkHTTPRETWrong` | RETRIEVE the &lt;CSEBase&gt; with an absolute `RET` header in the past -> expects REQUEST_TIMEOUT. |
    | 6 | `test_checkHTTPRETRelativeWrong` | RETRIEVE the &lt;CSEBase&gt; with a negative relative `RET` -> expects REQUEST_TIMEOUT. |
    | 7 | `test_checkHTTPRVIWrongInRequest` | RETRIEVE the &lt;CSEBase&gt; with an unsupported `RVI` version (`'1'`) -> expects RELEASE_VERSION_NOT_SUPPORTED. |
    | 8 | `test_createUnknownResourceTypeFail` | CREATE a resource with an unrecognized numeric type (`999`) and a non-standard JSON key -> expects BAD_REQUEST. |
    | 9 | `test_createNoResourceTypeFail` | CREATE a resource with `ty=None` (no type specified) -> expects BAD_REQUEST. |
    | 10 | `test_sendNotificationToOwnCSEFail` | NOTIFY the &lt;CSEBase&gt; itself with a generic `m2m:sgn` body -> expects BAD_REQUEST. |
    | 11 | `test_createEmpty` | CREATE an &lt;AE&gt; with `None` as the body -> expects BAD_REQUEST. |
    | 12 | `test_updateEmpty` | UPDATE the &lt;CSEBase&gt; with `None` as the body -> expects BAD_REQUEST. |
    | 13 | `test_createAlphaResourceType` | CREATE a resource passing a non-numeric string (`'wrong'`) as the resource type -> expects BAD_REQUEST. |
    | 14 | `test_createWithWrongResourceType` | CREATE a resource where the JSON body key (`m2m:ae`) doesn't match the declared resource type (`T.CNT`) -> expects BAD_REQUEST. |
    | 15 | `test_checkHTTPmissingOriginator` | RETRIEVE the &lt;CSEBase&gt; with no originator at all (HTTP binding only) -> expects BAD_REQUEST. |
    | 16 | `test_checkResponseOT` | RETRIEVE the &lt;CSEBase&gt; with an `OT` (originating timestamp) header -> expects OK; checks the response echoes a valid, parseable ISO 8601 `OT` header. |
    | 17 | `test_checkTargetRVI` | CREATE an &lt;AE&gt; declaring `srv=['2']` (release version 2) -> expects CREATED; NOTIFY the &lt;AE&gt; -> expects OK; checks the resulting notification's `X-M2M-RVI` header matches the target's declared release version (`'2'`), not the sender's; DELETE the &lt;AE&gt; -> expects DELETED. |
    | 18 | `test_validateListFail` | CREATE an &lt;AE&gt; with a `lbl` list containing a mixed-type element (a string and an integer) -> expects BAD_REQUEST. |
    | 19 | `test_resourceWithoutRN` | CREATE a &lt;CNT&gt; with no `rn` -> expects CREATED (CSE auto-generates a name); RETRIEVE it by the generated `rn` -> expects OK; DELETE it -> expects DELETED. |
    | 20 | `test_subWithoutRN` | CREATE a &lt;SUB&gt; with no `rn` -> expects CREATED; RETRIEVE it by the generated `rn` -> expects OK; DELETE it -> expects DELETED. |
    | 21 | `test_createAEContentTypeWithSpacesHeader` | CREATE an &lt;AE&gt; (HTTP only) with a `Content-Type` header containing extra spaces around the `ty` parameter -> expects CREATED; DELETE it -> expects DELETED. |
    | 22 | `test_retrieveCSEwithResourceTypeFail` | RETRIEVE the &lt;CSEBase&gt; with a `Content-Type` header that includes a `ty` parameter (not allowed on RETRIEVE) -> expects BAD_REQUEST. |
    | 23 | `test_tokenValidationFail` | CREATE a &lt;CNT&gt; -> expects CREATED; UPDATE its `lbl` with invalid token values in sequence (double space, tab, newline, carriage return, empty string) -> each expects BAD_REQUEST; cleanup DELETE (result not checked). |
    | 24 | `test_wrongRCNinUpdateFail` | CREATE a &lt;CNT&gt; -> expects CREATED; UPDATE it with `rcn=2` (an RCN not valid for UPDATE) -> expects BAD_REQUEST; DELETE the &lt;CNT&gt; -> expects DELETED. |
    | 25 | `test_partialRetrieveCSEBaseSingle` | RETRIEVE the &lt;CSEBase&gt; with `atrl=rn` (partial attribute retrieve) -> expects OK; checks only `rn` is present and `ri` is absent. |
    | 26 | `test_partialRetrieveCSEBaseMultiple` | RETRIEVE the &lt;CSEBase&gt; with `atrl=rn+ty` -> expects OK; checks `rn`/`ty` present and `ri` absent. |
    | 27 | `test_partialDeleteCSEBaseFail` | DELETE the &lt;CSEBase&gt; with an `atrl` query parameter (partial delete not supported) -> expects BAD_REQUEST. |
    | 28 | `test_partialRetrieveCSEBaseWrongRcnFail` | RETRIEVE the &lt;CSEBase&gt; with `atrl=rn` combined with an incompatible `rcn=2` -> expects BAD_REQUEST. |
    | 29 | `test_partialRetrieveCSEBaseWrongAttributeFail` | RETRIEVE the &lt;CSEBase&gt; with `atrl=mni` (an attribute that doesn't exist on CSEBase) -> expects BAD_REQUEST. |
    | 30 | `test_partialRetrieveCSEBaseROAttribute` | RETRIEVE the &lt;CSEBase&gt; with `atrl=ctm` (a valid read-only attribute) -> expects OK; checks `ri` is absent (only requested attribute returned). |
    | 31 | `test_partialRetrieveCSingleOptionalAttribute` | CREATE a &lt;CNT&gt; -> expects CREATED; RETRIEVE it with `atrl=mni` -> expects OK, checks `ri` absent; DELETE the &lt;CNT&gt; -> expects DELETED. |
    | 32 | `test_partialRetrieveFCNT` | CREATE a &lt;FCNT&gt; (`cod:lock`) -> expects CREATED; RETRIEVE it with `atrl=lock` -> expects OK, checks `ri` absent but `lock` present; DELETE the &lt;FCNT&gt; -> expects DELETED. |
    | 33 | `test_notifyAE` | CREATE an &lt;AE&gt; with a notification `poa` -> expects CREATED; NOTIFY the &lt;AE&gt; with a generic `m2m:sgn` body (testing that extra query arguments aren't required) -> expects OK; checks a notification and its headers were received and no extraneous arguments were parsed; DELETE the &lt;AE&gt; -> expects DELETED. |
    | 34 | `test_noResponseRetrieve` | CREATE a &lt;CNT&gt; -> expects CREATED; RETRIEVE it with `rt=noResponse` -> expects NO_CONTENT with an empty body; DELETE the &lt;CNT&gt; -> expects DELETED. |
    | 35 | `test_noResponseCreate` | CREATE a &lt;CNT&gt; with `rt=noResponse` -> expects NO_CONTENT with an empty body; DELETE the &lt;CNT&gt; -> expects DELETED. |
    | 36 | `test_noResponseUpdate` | CREATE a &lt;CNT&gt; -> expects CREATED; UPDATE it with `rt=noResponse` -> expects NO_CONTENT with an empty body; DELETE the &lt;CNT&gt; -> expects DELETED. |
    | 37 | `test_noResponseDelete` | CREATE a &lt;CNT&gt; -> expects CREATED; DELETE it with `rt=noResponse` -> expects NO_CONTENT with an empty body. |

??? note "`testALST.py` — AE Contact List (ALST) resource type handling (2 tests)"

    | # | Test Method | Requests Performed |
    |---|---|---|
    | 1 | `test_createALSTFail` | CREATE an &lt;ALST&gt; (AEContactList) directly under the &lt;CSEBase&gt; via a normal request -> expects OPERATION_NOT_ALLOWED (ALST can only be created internally, not via request). |
    | 2 | `test_deleteALSTFail` | DELETE the &lt;ALST&gt; under the &lt;CSEBase&gt; -> expects OPERATION_NOT_ALLOWED. |
