# Discovery, Requests & Expiration

*4 test modules, 100 test cases. [&larr; Back to overview](index.md)*

??? note "`testDiscovery.py` — Resource discovery requests (filter criteria, result content, etc.) (59 tests)"

    | # | Test Method | Requests Performed |
    |---|---|---|
    | 1 | `test_retrieveUnknownResource` | RETRIEVE a non-existent resource path -> expects NOT_FOUND. |
    | 2 | `test_discoverUnknownResource` | RETRIEVE (discovery, `fu=1`) a non-existent resource path -> expects NOT_FOUND. |
    | 3 | `test_discoverUnknownAttribute` | RETRIEVE with an unrecognized query parameter (`xxx=yyy`) -> expects BAD_REQUEST. |
    | 4 | `test_retrieveCNIwithWrongSZB` | RETRIEVE with `rcn=childResourceReferences` and an invalid negative `szb` (size below) -> expects BAD_REQUEST. |
    | 5 | `test_discoverCNTunderAERCN6` | Discover &lt;CNT&gt;s under &lt;AE&gt; with `fu=1`, `rcn=childResourceReferences`, `ty=CNT` -> expects OK; checks 2 distinct reference entries with correct `nm`/`typ`/`val` matching the two created containers. |
    | 6 | `test_discoveryCNTunderAERCN11` | Discover &lt;CNT&gt;s with `rcn=discoveryResultReferences` -> expects OK; checks 2 distinct URIs in `m2m:uril` matching the containers. |
    | 7 | `test_discoverCNTunderAEWrongRCN1` | Discover with `rcn=attributes` (not valid for discovery) -> expects BAD_REQUEST. |
    | 8 | `test_discoverCNTunderAEWrongRCN4` | Discover with `rcn=attributesAndChildResources` (invalid for discovery) -> expects BAD_REQUEST. |
    | 9 | `test_discoverCNTunderAEWrongRCN5` | Discover with `rcn=attributesAndChildResourceReferences` (invalid for discovery) -> expects BAD_REQUEST. |
    | 10 | `test_discoverCNTunderAEWrongRCN8` | Discover with `rcn=childResources` (invalid for discovery) -> expects BAD_REQUEST. |
    | 11 | `test_discoverCNTunderAEWrongRCN9` | Discover with `rcn=modifiedAttributes` (invalid for discovery) -> expects BAD_REQUEST. |
    | 12 | `test_retrieveCNTunderAERCN6` | RETRIEVE (non-discovery) with `rcn=childResourceReferences`, `ty=CNT` -> expects OK; checks 2 distinct reference entries. |
    | 13 | `test_retrieveCNTunderAERCN1` | RETRIEVE with `rcn=attributes`, `ty=CNT` -> expects OK; checks only the &lt;AE&gt;'s own attributes are returned (no children). |
    | 14 | `test_retrieveCNTunderAERCN4` | RETRIEVE with `rcn=attributesAndChildResources`, `ty=CNT` -> expects OK; checks the &lt;AE&gt;'s attributes plus 2 full child &lt;CNT&gt; resources. |
    | 15 | `test_retrieveCNTunderAERCN5` | RETRIEVE with `rcn=attributesAndChildResourceReferences`, `ty=CNT` -> expects OK; checks &lt;AE&gt; attributes plus 2 child references (`ch`). |
    | 16 | `test_retrieveCNTunderAERCN8` | RETRIEVE with `rcn=childResources`, `ty=CNT` -> expects OK; checks &lt;AE&gt;'s own attributes are NOT present, but 2 full child &lt;CNT&gt; resources are. |
    | 17 | `test_retrieveCNTunderAEWrongRCN9` | RETRIEVE (non-discovery) with `rcn=modifiedAttributes` (invalid for plain RETRIEVE) -> expects BAD_REQUEST. |
    | 18 | `test_retrieveCNTunderAEWrongRCN11` | RETRIEVE (non-discovery) with `rcn=discoveryResultReferences` (invalid for plain RETRIEVE) -> expects BAD_REQUEST. |
    | 19 | `test_retrieveCNTunderCSE` | RETRIEVE the &lt;CSEBase&gt; with `rcn=childResources`, `ty=CNT` -> expects OK; checks 2 child &lt;CNT&gt; resources. |
    | 20 | `test_retrieveCINunderAE` | RETRIEVE the &lt;AE&gt; with `rcn=childResources`, `ty=CIN` -> expects OK; checks 10 &lt;CIN&gt; resources (5 per container) are returned. |
    | 21 | `test_retrieveCINbyLBLunderAE` | RETRIEVE with `rcn=childResources`, `lbl=tag:0` -> expects OK; checks exactly 2 matching &lt;CIN&gt;s (one per container) with that label. |
    | 22 | `test_retrieveCNTbyCNIunderAE` | RETRIEVE with `rcn=childResources`, `cni=5` (filter containers by child count) -> expects OK; checks 2 matching &lt;CNT&gt;s, both with `cni=5`. |
    | 23 | `test_retrieveCNTbyCNIunderAEEmpty` | RETRIEVE with `cni=10` (no container has that count) -> expects OK; checks no matching &lt;CNT&gt;s. |
    | 24 | `test_retrieveCNTbyCNIunderAEEmpty2` | Same `cni=10` filter but with `rcn=childResourceReferences` -> expects OK; checks an empty `rrf` list. |
    | 25 | `test_retrieveCNTorCINunderAE` | RETRIEVE with `ty=CNT+CIN` (OR via `+` syntax) -> expects OK; checks 12 total references (2 CNT + 10 CIN). |
    | 26 | `test_retrieveCNTorCINunderAE2` | Same OR-type filter but using repeated `ty=` query params instead of `+` -> expects OK; checks the same 12 references. |
    | 27 | `test_retrieveCINandLBLunderAE` | RETRIEVE with `ty=CIN&lbl=tag:0` (AND) -> expects OK; checks exactly 2 matching &lt;CIN&gt;s. |
    | 28 | `test_retrieveCINandLBLunderAE2` | RETRIEVE with `ty=CIN&lbl=tag:0+tag:1` (multi-value label OR within AND type filter) -> expects OK; checks 4 matching &lt;CIN&gt;s. |
    | 29 | `test_retrieveCNTorLBLunderAE` | RETRIEVE with `ty=CNT&lbl=tag:0&fo=OR` (explicit OR across filter criteria) -> expects OK; checks 4 matches (2 CNT + 2 CIN matching by label). |
    | 30 | `test_retrieveWithCRBunderAE` | RETRIEVE with `crb` (created-before) set to the pre-setup timestamp -> expects OK with 0 results; RETRIEVE with `crb` set to the post-setup timestamp -> expects OK with results present. |
    | 31 | `test_retrieveWithCRAunderAE` | RETRIEVE with `cra` (created-after) set to the pre-setup timestamp -> expects OK with results present; RETRIEVE with `cra` set to the post-setup timestamp -> expects OK with 0 results. |
    | 32 | `test_retrieveCNIwithCTYunderAE` | RETRIEVE with `cty=text/plain:0` (content-type filter) -> expects OK; checks exactly 5 matches. |
    | 33 | `test_retrieveCNIwithSZBunderAE` | RETRIEVE with `szb=100` (size below) -> expects OK; checks all 10 &lt;CIN&gt;s match. |
    | 34 | `test_retrieveCNIwithSZAunderAE` | RETRIEVE with `sza=3` (size above) -> expects OK; checks all 10 &lt;CIN&gt;s match. |
    | 35 | `test_retrieveCNIwithMSunderAE` | RETRIEVE with `ms` (modified since) set to the pre-setup timestamp -> expects OK with results present. |
    | 36 | `test_retrieveCNIwithUSunderAE` | RETRIEVE with `us` (unmodified since) set to the pre-setup timestamp -> expects OK with 0 results; with the post-setup timestamp -> expects OK with results present. |
    | 37 | `test_retrieveCNIwithEXBunderAE` | RETRIEVE with `exb` (expire before) set to the pre-setup timestamp -> expects OK with 0 results. |
    | 38 | `test_retrieveCNIwithEXAunderAE` | RETRIEVE with `exa` (expire after) set to the pre-setup timestamp -> expects OK with results present. |
    | 39 | `test_retrieveCNTunderAEStructured` | RETRIEVE with `drt=structured` -> expects OK; checks the returned reference URIs end with structured paths (`/AE/CNT`). |
    | 40 | `test_retrieveCNTunderAEUnstructured` | RETRIEVE with `drt=unstructured` -> expects OK; checks the returned reference URIs do NOT end with structured paths. |
    | 41 | `test_rcn4WithDifferentFUs` | RETRIEVE with `rcn=attributesAndChildResources` and no `fu` -> expects OK with full nested structure (2 CNT, each with 5 CIN); with `fu=1` (discovery) -> expects BAD_REQUEST (rcn=4 invalid for discovery); with `fu=2` (retrieve, explicit) -> expects OK with the same nested structure. |
    | 42 | `test_appendArp` | CREATE a nested &lt;CNT&gt; (`arpCnt`) under the first container -> expects CREATED; RETRIEVE with `rcn=childResources`, `lbl=cntLbl`, `arp=arpCnt` (attribute relative path) -> expects OK, checks the result is the nested resource itself; DELETE it -> expects DELETED. |
    | 43 | `test_createCNTwithRCN9` | CREATE a &lt;CNT&gt; (with `mni`/`lbl`) using `rcn=modifiedAttributes` -> expects CREATED; checks `mni`/`lbl` are absent from the response (not modified by CSE) but `st`/`ri`/`ct`/`lt` (CSE-set attributes) are present. |
    | 44 | `test_updateCNTwithRCN9` | UPDATE the &lt;CNT&gt; (`mni`/`lbl`) using `rcn=modifiedAttributes` -> expects UPDATED; checks `mni`/`lbl` absent but `st`/`lt` present. |
    | 45 | `test_updateCNTwithWrongRCN2` | UPDATE the &lt;CNT&gt; using `rcn=hierarchicalAddress` (invalid for UPDATE) -> expects BAD_REQUEST. |
    | 46 | `test_createCNTwithRCN0` | CREATE a &lt;CNT&gt; using `rcn=nothing` -> expects CREATED; checks the response body is empty/`None`. |
    | 47 | `test_retrieveWithWrongArgument` | RETRIEVE with an unrecognized query parameter alongside `rcn=attributes` -> expects BAD_REQUEST. |
    | 48 | `test_retrieveWithWrongFU` | RETRIEVE with an invalid `fu` value -> expects BAD_REQUEST. |
    | 49 | `test_retrieveWithWrongDRT` | RETRIEVE with an invalid `drt` value -> expects BAD_REQUEST. |
    | 50 | `test_retrieveWithWrongFO` | RETRIEVE with an invalid `fo` value -> expects BAD_REQUEST. |
    | 51 | `test_retrieveMgmtObjsRCN8` | RETRIEVE the &lt;NOD&gt; with `rcn=childResources`, `ty=MGMTOBJ` -> expects OK; checks the 2 &lt;BAT&gt;s and 1 &lt;MEM&gt; are returned as separate typed lists (`m2m:bat`, `m2m:mem`). |
    | 52 | `test_retrieveCINmatchLabel` | RETRIEVE the &lt;CNT&gt; with `rcn=childResources`, `con=a*` (wildcard content match) -> expects OK; checks all 5 matching &lt;CIN&gt;s are returned. |
    | 53 | `test_createCNTwithRCN2` | CREATE a &lt;CNT&gt; using `rcn=hierarchicalAddress` -> expects CREATED; checks `m2m:uri` is present, and (HTTP only) the `Content-Location` header matches it. |
    | 54 | `test_createCNTwithRCN3` | CREATE a &lt;CNT&gt; using `rcn=hierarchicalAddressAttributes` -> expects CREATED; checks `m2m:rce/uri` is present, and (HTTP only) the `Content-Location` header matches it. |
    | 55 | `test_retrieveUnderCNTRCN8` | CREATE a fresh empty &lt;CNT&gt; -> expects CREATED; RETRIEVE it with `rcn=childResources` -> expects OK, checks the child list is empty; DELETE it -> expects DELETED. |
    | 56 | `test_retrieveUnderCNTRCN6` | CREATE a fresh empty &lt;CNT&gt; -> expects CREATED; RETRIEVE it with `rcn=childResourceReferences` -> expects OK, checks `rrf` is empty; DELETE it with the same `rcn` -> expects DELETED, checks the deletion response's `rrf` is also empty. |
    | 57 | `test_retrieveUnderCNTRCN5` | CREATE a fresh empty &lt;CNT&gt; -> expects CREATED; RETRIEVE it with `rcn=attributesAndChildResourceReferences` -> expects OK, checks `ch` is empty; DELETE it -> expects DELETED. |
    | 58 | `test_updateCNTwithRCN12Fail` | CREATE a &lt;CNT&gt; -> expects CREATED; UPDATE it using `rcn=permissions` (rcn=12, not valid for UPDATE) -> expects BAD_REQUEST; DELETE it -> expects DELETED. |
    | 59 | `test_retrieveCNTwithRCN12` | CREATE a &lt;CNT&gt; and a nested child &lt;CNT&gt; -> expects CREATED; RETRIEVE the parent with `rcn=permissions` -> expects OK; DELETE the parent -> expects DELETED. |

??? note "`testREQ.py` — Request (&lt;REQ&gt;) resource functionality (async/non-blocking requests) (26 tests)"

    | # | Test Method | Requests Performed |
    |---|---|---|
    | 1 | `test_createREQFail` | CREATE a &lt;REQ&gt; manually under &lt;CSEBase&gt; -> expects OPERATION_NOT_ALLOWED (REQ resources are CSE-internal only). |
    | 2 | `test_retrieveCSENBSynch` | RETRIEVE the &lt;CSEBase&gt; with `rt=nonBlockingRequestSynch` and `rp` (result persistence) -> expects ACCEPTED_NON_BLOCKING_REQUEST_SYNC, checks `m2m:uri`; RETRIEVE the resulting &lt;REQ&gt; resource -> expects OK; extensively checks its attributes (`st` absent, `lbl`, `op=RETRIEVE`, `tg`, `pc` absent, `org`, `rid` matches original request ID, `mi.rt.rtv`, `mi.rp`, `ors.rqi`/`rsc=OK`/`pc.m2m:cb.ty=CSEBase`). |
    | 3 | `test_retrieveCSENBSynchValidateREQ` | RETRIEVE the &lt;CSEBase&gt; non-blocking-sync with a delayed `OET` -> expects ACCEPTED_NON_BLOCKING_REQUEST_SYNC; immediately RETRIEVE the &lt;REQ&gt; -> expects OK, checks `rs=PENDING` and `ors.rsc=ACCEPTED`; after a delay, RETRIEVE the &lt;REQ&gt; again -> expects OK, checks `lt` advanced and `rs=COMPLETED`. |
    | 4 | `test_testResultPersistence` | RETRIEVE the &lt;CSEBase&gt; non-blocking-sync -> expects ACCEPTED_NON_BLOCKING_REQUEST_SYNC; RETRIEVE the &lt;REQ&gt; shortly after -> expects OK; sleep past its expiration; RETRIEVE the &lt;REQ&gt; again -> expects NOT_FOUND (auto-deleted after persistence window). |
    | 5 | `test_retrieveCSENBSynchMissingRP` | RETRIEVE the &lt;CSEBase&gt; non-blocking-sync without `rp` -> expects ACCEPTED_NON_BLOCKING_REQUEST_SYNC; RETRIEVE the &lt;REQ&gt; -> expects OK (default `rp` applied by the CSE). |
    | 6 | `test_retrieveCSENBSynchWrongRT` | RETRIEVE the &lt;CSEBase&gt; with an invalid `rt` value (`99999`) -> expects BAD_REQUEST. |
    | 7 | `test_retrieveCSENBSynchExpireRequest` | RETRIEVE the &lt;CSEBase&gt; non-blocking-sync -> expects ACCEPTED_NON_BLOCKING_REQUEST_SYNC; sleep past the expiration window without checking sooner; RETRIEVE the &lt;REQ&gt; -> expects NOT_FOUND. |
    | 8 | `test_retrieveUnknownNBSynch` | RETRIEVE a non-existent resource non-blocking-sync -> expects ACCEPTED_NON_BLOCKING_REQUEST_SYNC; RETRIEVE the &lt;REQ&gt; -> expects OK, checks `ors.rsc=NOT_FOUND` (the underlying failed operation result recorded in the request). |
    | 9 | `test_retrieveCNTNBFlex` | RETRIEVE the &lt;CSEBase&gt; with `rt=flexBlocking` -> expects one of OK/ACCEPTED_NON_BLOCKING_REQUEST_SYNC/ACCEPTED_NON_BLOCKING_REQUEST_ASYNC (server decides dynamically); result not otherwise checked. |
    | 10 | `test_retrieveCNTNBFlexIntegerDuration` | Same flex-blocking RETRIEVE but `rp` given as a plain integer duration -> expects one of the same 3 outcomes. |
    | 11 | `test_retrieveCNTNBFlexWrongDuration` | Same flex-blocking RETRIEVE but `rp` given as an invalid non-numeric string (`xxx`) -> expects BAD_REQUEST. |
    | 12 | `test_createCNTNBSynch` | CREATE a &lt;CNT&gt; non-blocking-sync -> expects ACCEPTED_NON_BLOCKING_REQUEST_SYNC; RETRIEVE the &lt;REQ&gt; -> expects OK, checks `pc` (original content) is set, and `ors.rsc=CREATED`/`ors.pc.m2m:cnt.ty=CNT`/`ors.rqi` match. |
    | 13 | `test_updateCNTNBSynch` | UPDATE the &lt;CNT&gt; (`lbl`) non-blocking-sync -> expects ACCEPTED_NON_BLOCKING_REQUEST_SYNC; RETRIEVE the &lt;REQ&gt; -> expects OK, checks `pc` set, `ors.rsc=UPDATED`, and `ors.pc.m2m:cnt.lbl` contains the new label. |
    | 14 | `test_deleteCNTNBSynch` | DELETE the &lt;CNT&gt; non-blocking-sync -> expects ACCEPTED_NON_BLOCKING_REQUEST_SYNC; RETRIEVE the &lt;REQ&gt; -> expects OK, checks `ors.rsc=DELETED` and `ors.rqi` matches. |
    | 15 | `test_retrieveCSENBSynchWithRET` | RETRIEVE the &lt;CSEBase&gt; non-blocking-sync with an explicit `RET` header (no `rp` query param) -> expects ACCEPTED_NON_BLOCKING_REQUEST_SYNC; RETRIEVE the &lt;REQ&gt; -> expects OK. |
    | 16 | `test_retrieveCSENBSynchWithRETshort` | Same but with a very short `RET` -> expects ACCEPTED_NON_BLOCKING_REQUEST_SYNC initially; sleep past expiration; RETRIEVE the &lt;REQ&gt; -> expects NOT_FOUND. |
    | 17 | `test_retrieveCSENBSynchWithVSI` | RETRIEVE the &lt;CSEBase&gt; non-blocking-sync with a `VSI` (vendor information) header -> expects ACCEPTED_NON_BLOCKING_REQUEST_SYNC; RETRIEVE the &lt;REQ&gt; -> expects OK, checks `mi.vsi` echoes the vendor info. |
    | 18 | `test_retrieveCSENBAsynch` | RETRIEVE the &lt;CSEBase&gt; with `rt=nonBlockingRequestAsynch` and an `RTU` (notification target) header -> expects ACCEPTED_NON_BLOCKING_REQUEST_ASYNC; checks a result notification (`m2m:rsp`) is received with `rsc=OK`, `to`/`fr`, `pc.m2m:cb.ty=CSEBase`, and matching `rqi`. |
    | 19 | `test_retrieveCSENBAsynch2` | Same async RETRIEVE but with two `RTU` URLs (duplicate notification targets) -> expects ACCEPTED_NON_BLOCKING_REQUEST_ASYNC; checks the same `m2m:rsp` notification content. |
    | 20 | `test_retrieveCSENBAsynchEmptyRTU` | Same async RETRIEVE but with an empty `RTU` header -> expects ACCEPTED_NON_BLOCKING_REQUEST_ASYNC; checks NO notification is sent. |
    | 21 | `test_retrieveCSENBAsynchNoRTU` | Same async RETRIEVE with no `RTU` header at all (falls back to the AE's `poa`) -> expects ACCEPTED_NON_BLOCKING_REQUEST_ASYNC; checks a notification IS still received via the AE's point-of-access. |
    | 22 | `test_retrieveUnknownNBAsynch` | RETRIEVE a non-existent resource async -> expects ACCEPTED_NON_BLOCKING_REQUEST_ASYNC; checks the resulting notification's `rsc=NOT_FOUND` and matching `rqi`. |
    | 23 | `test_createCNTNBAsynch` | CREATE a &lt;CNT&gt; async -> expects ACCEPTED_NON_BLOCKING_REQUEST_ASYNC; checks the resulting notification's `rsc=CREATED`, `pc.m2m:cnt.ty=CNT`, matching `rqi`. |
    | 24 | `test_updateCNTNBAsynch` | UPDATE the &lt;CNT&gt; (`lbl`) async -> expects ACCEPTED_NON_BLOCKING_REQUEST_ASYNC; checks the resulting notification's `rsc=UPDATED`, `to`/`fr`, `pc.m2m:cnt.ty=CNT`/`lbl` contains the new label, matching `rqi`. |
    | 25 | `test_deleteCNTNBASynch` | DELETE the &lt;CNT&gt; async -> expects ACCEPTED_NON_BLOCKING_REQUEST_ASYNC; checks the resulting notification's `rsc=DELETED` and matching `rqi`. |
    | 26 | `test_deleteREQFail` | RETRIEVE the &lt;CSEBase&gt; non-blocking-sync with a delayed `OET` (request still pending) -> expects ACCEPTED_NON_BLOCKING_REQUEST_SYNC; attempt to DELETE the resulting &lt;REQ&gt; (recall) -> expects UNABLE_TO_RECALL_REQUEST. |

??? note "`testExpiration.py` — Resource expiration handling (9 tests)"

    | # | Test Method | Requests Performed |
    |---|---|---|
    | 1 | `test_expireCNT` | CREATE a &lt;CNT&gt; with `et` (expiration time) set 2 seconds in the future -> expects CREATED; sleeps past expiration; RETRIEVE the &lt;CNT&gt; -> expects NOT_FOUND (resource was expired/removed by the CSE). |
    | 2 | `test_expireCNTAndCIN` | CREATE a &lt;CNT&gt; with a near-future `et` -> expects CREATED; CREATE 5 &lt;CIN&gt;s under it in a loop -> each expects CREATED; RETRIEVE the last &lt;CIN&gt; -> expects OK; sleep past expiration; RETRIEVE the &lt;CNT&gt; -> expects NOT_FOUND; RETRIEVE the &lt;CIN&gt; -> expects NOT_FOUND (children expire with the parent). |
    | 3 | `test_createCNTWithToLargeET` | CREATE a &lt;CNT&gt; with an excessively far-future `et` -> expects CREATED, but checks the CSE silently corrected/capped the returned `et` to less than the requested value; DELETE the &lt;CNT&gt; -> expects DELETED. |
    | 4 | `test_createCNTExpirationInThePast` | CREATE a &lt;CNT&gt; with `et` set 1 minute in the past -> expects BAD_REQUEST. |
    | 5 | `test_updateCNTWithEtNull` | CREATE a &lt;CNT&gt; with `et` set 1 minute in the future -> expects CREATED; UPDATE it setting `et` to `None` (remove expiration) -> expects UPDATED; DELETE the &lt;CNT&gt; -> expects DELETED. |
    | 6 | `test_expireCNTViaMIA` | CREATE a &lt;CNT&gt; with `mia` (max instance age) set to a short delay -> expects CREATED, checks `mia` in the response; CREATE 5 &lt;CIN&gt;s under it -> each expects CREATED; sleep past the MIA delay; RETRIEVE the &lt;CNT&gt; -> expects OK, checks `cni` (child count) is now 0 (children expired via MIA, parent unaffected); DELETE the &lt;CNT&gt; -> expects DELETED. |
    | 7 | `test_expireCNTViaMIALarge` | CREATE a &lt;CNT&gt; with an excessively large `mia` -> expects CREATED; CREATE 5 &lt;CIN&gt;s, each checked to have an `et` below the too-large threshold -> each expects CREATED; sleep; RETRIEVE the &lt;CNT&gt; -> expects OK, checks all 5 children are still present (no premature expiration); DELETE the &lt;CNT&gt; -> expects DELETED. |
    | 8 | `test_expireFCNTViaMIA` | CREATE a &lt;FCNT&gt; (`cod:tempe`) with `mia` set short -> expects CREATED, checks `mia`/`cni`/`cbs`; UPDATE it 5 times in a loop (each creating a FlexContainerInstance) -> each expects UPDATED, checks `cni` grows to 6; sleep past MIA; RETRIEVE the &lt;FCNT&gt; -> expects OK, checks `cni`/`cbs` reset to 0 (instances expired); DELETE the &lt;FCNT&gt; -> expects DELETED. |
    | 9 | `test_expirationLaterThanParents` | CREATE a parent &lt;CNT&gt; with `et` 1 minute in the future -> expects CREATED; CREATE a child &lt;CNT&gt; under it requesting `et` 2 minutes in the future (later than the parent) -> expects CREATED, but checks the child's `et` was clamped down to match the parent's `et`; DELETE the child &lt;CNT&gt; -> expects DELETED. |

??? note "`testUpperTester.py` — Upper Tester interface functionality (6 tests)"

    | # | Test Method | Requests Performed |
    |---|---|---|
    | 1 | `test_checkStatus` | POST a raw HTTP request to the Upper Tester endpoint with `X-M2M-UTCMD: Status` -> expects HTTP 200 with a `2000` result-code header and a CSE status value (STOPPED/STARTING/RUNNING/STOPPING/RESETTING) in the response header. |
    | 2 | `test_performReset` | RETRIEVE the &lt;CSEBase&gt; -> expects OK, records its `ct` (creation time); POST a UT command `Reset` -> expects HTTP 200/2000; RETRIEVE the &lt;CSEBase&gt; again -> expects OK, checks the new `ct` is later than the old one (CSE was actually reset/recreated). |
    | 3 | `test_enableShortRequestExpiration` | POST a UT command `enableShortRequestExpiration 5` -> expects HTTP 200/2000 with a non-empty response header. |
    | 4 | `test_disableShortRequestExpiration` | POST a UT command `disableShortRequestExpiration` -> expects HTTP 200/2000. |
    | 5 | `test_enableShortResourceExpiration` | POST a UT command `enableShortResourceExpiration 5` -> expects HTTP 200/2000 with a non-empty response header. |
    | 6 | `test_disableShortResourceExpiration` | POST a UT command `disableShortResourceExpiration` -> expects HTTP 200/2000. |
