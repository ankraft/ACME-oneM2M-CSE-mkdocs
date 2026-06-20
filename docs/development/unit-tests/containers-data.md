# Resource Tree: Containers & Data

*8 test modules, 190 test cases. [&larr; Back to overview](index.md)*

??? note "`testCNT.py` — Container (&lt;CNT&gt;) resource lifecycle and attribute handling (22 tests)"

    | # | Test Method | Requests Performed |
    |---|---|---|
    | 1 | `test_createCNT` | CREATE a &lt;CNT&gt; under the &lt;AE&gt; -> expects CREATED. |
    | 2 | `test_retrieveCNT` | RETRIEVE the &lt;CNT&gt; -> expects OK. |
    | 3 | `test_retrieveCNTWithWrongOriginator` | RETRIEVE the &lt;CNT&gt; with an unauthorized originator -> expects ORIGINATOR_HAS_NO_PRIVILEGE. |
    | 4 | `test_attributesCNT` | RETRIEVE the &lt;CNT&gt; -> expects OK; checks `ty`, `pi`, `rn`, `ct`, `lt`, `et`, `st`, `cr` (absent), `cbs=0`, `cni=0`, `lbl` (absent). |
    | 5 | `test_updateCNT` | UPDATE the &lt;CNT&gt; setting `lbl`, `mni`, `mbs` -> expects UPDATED; RETRIEVE it again -> expects OK, checks all updated attributes and that `st` (state tag) incremented to 1. |
    | 6 | `test_updateCNTTy` | UPDATE the &lt;CNT&gt; attempting to change `ty` -> expects BAD_REQUEST. |
    | 7 | `test_updateCNTPi` | UPDATE the &lt;CNT&gt; attempting to change `pi` -> expects BAD_REQUEST. |
    | 8 | `test_updateCNTUnknownAttribute` | UPDATE the &lt;CNT&gt; with an unknown attribute -> expects BAD_REQUEST. |
    | 9 | `test_updateCNTWrongMNI` | UPDATE the &lt;CNT&gt; with an invalid negative `mni` -> expects BAD_REQUEST. |
    | 10 | `test_updateCNTempty` | UPDATE the &lt;CNT&gt; with an empty body -> expects UPDATED; checks the resource is still returned. |
    | 11 | `test_createCNTUnderCNT` | CREATE a child &lt;CNT&gt; under the existing &lt;CNT&gt; -> expects CREATED. |
    | 12 | `test_retrieveCNTUnderCNT` | RETRIEVE the nested &lt;CNT&gt; -> expects OK. |
    | 13 | `test_deleteCNTUnderCNT` | DELETE the nested &lt;CNT&gt; -> expects DELETED. |
    | 14 | `test_createCNTWithCreatorWrong` | CREATE a &lt;CNT&gt; with `cr` explicitly set to a wrong non-null value -> expects BAD_REQUEST. |
    | 15 | `test_createCNTWithCreator` | CREATE a &lt;CNT&gt; with `cr` set to `None` -> expects CREATED, checks `cr` auto-set to originator; RETRIEVE it -> expects OK, checks `cr` persists. |
    | 16 | `test_deleteCNTByUnknownOriginator` | DELETE the &lt;CNT&gt; with an unauthorized originator -> expects ORIGINATOR_HAS_NO_PRIVILEGE. |
    | 17 | `test_deleteCNTByAssignedOriginator` | DELETE the &lt;CNT&gt; with the correct originator -> expects DELETED. |
    | 18 | `test_createCNTUnderCSE` | CREATE a &lt;CNT&gt; directly under the &lt;CSEBase&gt; using the admin originator -> expects CREATED. |
    | 19 | `test_retrieveCNTUnderCSE` | RETRIEVE the &lt;CNT&gt; under &lt;CSEBase&gt; -> expects OK. |
    | 20 | `test_deleteCNTUnderCSE` | DELETE the &lt;CNT&gt; under &lt;CSEBase&gt; -> expects DELETED. |
    | 21 | `test_createCNTWithoutOriginator` | CREATE a &lt;CNT&gt; under &lt;CSEBase&gt; with no originator at all (HTTP binding only) -> expects something other than CREATED. |
    | 22 | `test_createCNTwithWrongTypeShortname` | CREATE a &lt;CNT&gt; whose JSON body uses an incorrect type short-name key (`'wrong'` instead of `'m2m:cnt'`) -> expects something other than CREATED. |

??? note "`testCIN.py` — ContentInstance (&lt;CIN&gt;) creation, retrieval, and constraints (22 tests)"

    | # | Test Method | Requests Performed |
    |---|---|---|
    | 1 | `test_createCIN` | CREATE a &lt;CIN&gt; under the &lt;CNT&gt; with `cnf` and `con` set -> expects CREATED. |
    | 2 | `test_retrieveCIN` | RETRIEVE the &lt;CIN&gt; -> expects OK. |
    | 3 | `test_attributesCIN` | RETRIEVE the &lt;CIN&gt; -> expects OK; checks `ty`, `pi`, `rn`, `ct`, `lt`, `et`, `st`, `cr` (absent), `cnf`, `con`, `cs` (&gt;0), and `acpi` (absent). |
    | 4 | `test_updateCINFail` | UPDATE the &lt;CIN&gt; -> expects OPERATION_NOT_ALLOWED (CIN is immutable). |
    | 5 | `test_createCINUnderAE` | CREATE a &lt;CIN&gt; directly under the &lt;AE&gt; (invalid parent) -> expects INVALID_CHILD_RESOURCE_TYPE. |
    | 6 | `test_createCINwithString` | CREATE a &lt;CIN&gt; with a string `con` -> expects CREATED; checks `con` matches. |
    | 7 | `test_createCINwithInteger` | CREATE a &lt;CIN&gt; with an integer `con` -> expects CREATED; checks `con` matches. |
    | 8 | `test_createCINwithFloat` | CREATE a &lt;CIN&gt; with a float `con` -> expects CREATED; checks `con` matches. |
    | 9 | `test_createCINwithBoolean` | CREATE a &lt;CIN&gt; with a boolean `con` -> expects CREATED; checks `con` matches. |
    | 10 | `test_createCINwithList` | CREATE a &lt;CIN&gt; with a list `con` -> expects CREATED; checks `con` matches. |
    | 11 | `test_createCINwithStructure` | CREATE a &lt;CIN&gt; with a dict/JSON `con` -> expects CREATED; checks `con` matches. |
    | 12 | `test_deleteCIN` | DELETE the &lt;CIN&gt; -> expects DELETED. |
    | 13 | `test_createCINWithCreatorWrong` | CREATE a &lt;CIN&gt; with `cr` (creator) explicitly set to a non-null wrong value -> expects BAD_REQUEST. |
    | 14 | `test_createCINWithCnfWrong1` | CREATE a &lt;CIN&gt; with malformed `cnf` (`'text'`, missing subtype/encoding) -> expects BAD_REQUEST. |
    | 15 | `test_createCINWithCnfWrong2` | CREATE a &lt;CIN&gt; with malformed `cnf` (`'text:0'`, missing subtype) -> expects BAD_REQUEST. |
    | 16 | `test_createCINWithCnfWrong3` | CREATE a &lt;CIN&gt; with malformed `cnf` (`'text/plain'`, missing encoding) -> expects BAD_REQUEST. |
    | 17 | `test_createCINWithCnfWrong4` | CREATE a &lt;CIN&gt; with malformed `cnf` (too many colon-separated parts) -> expects BAD_REQUEST. |
    | 18 | `test_createCINWithCnfWrong5` | CREATE a &lt;CIN&gt; with `cnf` using an invalid encoding value (`:9`) -> expects BAD_REQUEST. |
    | 19 | `test_createCINWithCreator` | CREATE a &lt;CIN&gt; with `cr` explicitly set to `None` -> expects CREATED, checks `cr` was auto-set to the originator; RETRIEVE it -> expects OK, checks `cr` persists. |
    | 20 | `test_createRetrieveCINWithDcnt` | CREATE a &lt;CIN&gt; with `dcnt=5` (deletion counter) -> expects CREATED, checks `dcnt`; RETRIEVE it 5 times in a loop -> each expects OK with `dcnt` decreasing by 1 each time; a final RETRIEVE -> expects NOT_FOUND (deleted after the counter reached 0). |
    | 21 | `test_createCINwithAcpi` | CREATE a &lt;CIN&gt; with an `acpi` attribute set -> expects BAD_REQUEST (CIN doesn't support direct ACP assignment). |
    | 22 | `test_createCINwithDgt` | CREATE a &lt;CIN&gt; with a `dgt` (data generation time) attribute -> expects CREATED; RETRIEVE it -> expects OK, checks `dgt` matches. |

??? note "`testCNT_CIN.py` — Combined Container/ContentInstance (&lt;CNT&gt;/&lt;CIN&gt;) interaction behavior (26 tests)"

    | # | Test Method | Requests Performed |
    |---|---|---|
    | 1 | `test_addCIN` | CREATE a &lt;CIN&gt; under the &lt;CNT&gt; (`mni=3`) -> expects CREATED, checks `con`/`cnf`; RETRIEVE the &lt;CNT&gt; -> expects OK, checks `cni=1`. |
    | 2 | `test_addMoreCIN` | CREATE 3 more &lt;CIN&gt;s sequentially -> each expects CREATED; after each, RETRIEVE the &lt;CNT&gt; -> expects OK, checks `cni` grows then caps at 3 (oldest evicted once `mni` is exceeded). |
    | 3 | `test_retrieveCNTLa` | RETRIEVE the &lt;CNT&gt;'s `la` -> expects OK, checks it's the most recent &lt;CIN&gt; (`con='dValue'`). |
    | 4 | `test_retrieveCNTOl` | RETRIEVE the &lt;CNT&gt;'s `ol` -> expects OK, checks it's the oldest retained &lt;CIN&gt; (`con='bValue'`). |
    | 5 | `test_changeCNTMni` | UPDATE the &lt;CNT&gt; setting `mni=1` -> expects UPDATED, checks `mni`/`cni=1`; RETRIEVE `la` and `ol` -> both expect OK and both now point to the single remaining &lt;CIN&gt;. |
    | 6 | `test_deleteCINCheckCNT` | CREATE a &lt;CIN&gt; -> expects CREATED; RETRIEVE the parent &lt;CNT&gt; to record `cni`/`cbs` -> expects OK; DELETE the &lt;CIN&gt; -> expects DELETED; RETRIEVE the &lt;CNT&gt; again -> expects OK, checks `cni`/`cbs` changed (decreased). |
    | 7 | `test_deleteCNT` | DELETE the &lt;CNT&gt; -> expects DELETED. |
    | 8 | `test_createCNTwithMBS` | CREATE a &lt;CNT&gt; with `mbs` set -> expects CREATED, checks `mbs`. |
    | 9 | `test_createCINexactSize` | CREATE a &lt;CIN&gt; whose `con` size exactly equals `mbs` -> expects CREATED. |
    | 10 | `test_createCINtooBig` | CREATE a &lt;CIN&gt; whose `con` size exceeds `mbs` by 1 -> expects NOT_ACCEPTABLE. |
    | 11 | `test_createCINsForCNTwithSize` | Fill the &lt;CNT&gt; with several &lt;CIN&gt;s up to `mbs` -> each expects CREATED; RETRIEVE `la` -> expects OK; CREATE one more &lt;CIN&gt; (forcing byte-size eviction) -> expects CREATED; RETRIEVE `la` again -> expects OK with new content; RETRIEVE the &lt;CNT&gt; -> expects OK, checks `cni=3`, `cbs=mbs`. |
    | 12 | `test_createCNTwithDISR` | CREATE a &lt;CNT&gt; with `disr=True` (disable retrieval) -> expects CREATED, checks `disr`; CREATE 5 &lt;CIN&gt;s under it -> each expects CREATED. |
    | 13 | `test_retrieveCINwithDISRFail` | RETRIEVE a specific &lt;CIN&gt; while `disr=True` -> expects OPERATION_NOT_ALLOWED. |
    | 14 | `test_retrieveLAwithDISRFail` | RETRIEVE the &lt;CNT&gt;'s `la` while `disr=True` -> expects OPERATION_NOT_ALLOWED. |
    | 15 | `test_retrieveOLwithDISRFail` | RETRIEVE the &lt;CNT&gt;'s `ol` while `disr=True` -> expects OPERATION_NOT_ALLOWED. |
    | 16 | `test_discoverCINwithDISRFail` | RETRIEVE a discovery query (`rcn=childResourceReferences`) on the &lt;CNT&gt; while `disr=True` -> expects OK, but checks the returned `rrf` list is empty (children hidden from discovery). |
    | 17 | `test_deleteCINwithDISRFail` | DELETE a specific &lt;CIN&gt; while `disr=True` -> expects OPERATION_NOT_ALLOWED. |
    | 18 | `test_deleteLAwithDISRFail` | DELETE the &lt;CNT&gt;'s `la` while `disr=True` -> expects OPERATION_NOT_ALLOWED. |
    | 19 | `test_updateCNTwithDISRFalse` | UPDATE the &lt;CNT&gt; setting `disr=False` -> expects UPDATED, checks `disr=False` and `cni=0` (existing children purged when retrieval is re-enabled). |
    | 20 | `test_updateCNTwithDISRNullFalse` | UPDATE the &lt;CNT&gt; setting `disr=None` -> expects UPDATED, checks `disr` absent; CREATE 5 &lt;CIN&gt;s -> each expects CREATED; UPDATE again setting `disr=False` -> expects UPDATED, checks `disr=False`; CREATE 5 more &lt;CIN&gt;s -> each expects CREATED. |
    | 21 | `test_retrieveCINwithDISRAllowed` | RETRIEVE a specific &lt;CIN&gt; while `disr=False` -> expects OK (retrieval allowed again). |
    | 22 | `test_deleteCNTwithCINandDisr` | CREATE a &lt;CNT&gt; with `disr=True` -> expects CREATED; CREATE 5 &lt;CIN&gt;s under it -> each expects CREATED; DELETE the &lt;CNT&gt; -> expects OPERATION_NOT_ALLOWED (children can't be auto-deleted while `disr=True`); UPDATE setting `disr=False` -> expects UPDATED; DELETE the &lt;CNT&gt; again -> expects DELETED. |
    | 23 | `test_autoDeleteCINnoNotifiction` | CREATE a &lt;CNT&gt; with `mni=3`; CREATE a &lt;SUB&gt; monitoring `deleteDirectChild` events -> both expect CREATED; CREATE 5 &lt;CIN&gt;s (exceeding `mni`, forcing automatic eviction) -> each expects CREATED; checks NO notification was sent for the CSE-internal automatic deletions. |
    | 24 | `test_createCNT5CIN` | CREATE a &lt;CNT&gt; with `mni=10`; CREATE 5 &lt;CIN&gt;s under it -> all expect CREATED. |
    | 25 | `test_deleteCNTOl` | RETRIEVE the &lt;CNT&gt; to record `cni`/`cbs`; RETRIEVE its `ol` -> expects OK; DELETE the `ol` -> expects DELETED; RETRIEVE the new `ol` -> expects OK, checks it's a different `ri`; RETRIEVE the &lt;CNT&gt; again -> expects OK, checks `cni`/`cbs` decreased accordingly. |
    | 26 | `test_deleteCNTLA` | Same pattern as `test_deleteCNTOl` but for `la` (latest) -> RETRIEVE, DELETE `la` -> expects DELETED, checks the new `la` differs and `cni`/`cbs` decreased. |

??? note "`testFCNT.py` — FlexContainer (&lt;FCNT&gt;) resource lifecycle and notifications (24 tests)"

    | # | Test Method | Requests Performed |
    |---|---|---|
    | 1 | `test_createFCNTWrongCND` | CREATE a &lt;FCNT&gt; (`cod:tempe`) with an invalid `cnd` (container definition) value -> expects BAD_REQUEST. |
    | 2 | `test_createFCNTWrongTypeShortname` | CREATE a &lt;FCNT&gt; using an incorrect type short-name JSON key (`'wrong'`) -> expects BAD_REQUEST. |
    | 3 | `test_createFCNT` | CREATE a &lt;FCNT&gt; (`cod:tempe`) with valid `cnd`, `curT0`, `unit`, `minVe`, `maxVe`, `steVe` -> expects CREATED. |
    | 4 | `test_retrieveFCNT` | RETRIEVE the &lt;FCNT&gt; -> expects OK. |
    | 5 | `test_retrieveFCNTWithWrongOriginator` | RETRIEVE the &lt;FCNT&gt; with an unauthorized originator -> expects ORIGINATOR_HAS_NO_PRIVILEGE. |
    | 6 | `test_attributesFCNT` | RETRIEVE the &lt;FCNT&gt; -> expects OK; checks `ty`, `pi`, `rn`, `ct`, `lt`, `et`, `cr` (absent), `cnd`, `curT0`, `tarTe` (absent), `unit`, `minVe`, `maxVe`, `steVe`, `st=0`. |
    | 7 | `test_updateFCNT` | UPDATE the &lt;FCNT&gt; setting `tarTe=5.0` -> expects UPDATED; RETRIEVE it again -> expects OK, checks `tarTe`, unchanged `curT0`, `st` incremented to 1, and `lt` &gt; `ct`. |
    | 8 | `test_updateFCNTwithCnd` | UPDATE the &lt;FCNT&gt; attempting to change `cnd` -> expects BAD_REQUEST. |
    | 9 | `test_updateFCNTwithWrongType` | UPDATE the &lt;FCNT&gt; setting `tarTe` to a string instead of a float -> expects BAD_REQUEST. |
    | 10 | `test_updateFCNTwithUnkownAttribute` | UPDATE the &lt;FCNT&gt; with an unknown attribute -> expects BAD_REQUEST. |
    | 11 | `test_createFCNTUnknown` | CREATE a &lt;FCNT&gt; with an unrecognized custom specialization (`cod:unknown`) and an unknown `cnd`/attribute -> expects BAD_REQUEST. |
    | 12 | `test_createCNTUnderFCNT` | CREATE a &lt;CNT&gt; under the &lt;FCNT&gt; -> expects CREATED. |
    | 13 | `test_deleteCNTUnderFCNT` | DELETE the nested &lt;CNT&gt; -> expects DELETED. |
    | 14 | `test_createFCNTUnderFCNT` | CREATE a nested &lt;FCNT&gt; (`cod:tempe`) under the existing &lt;FCNT&gt; -> expects CREATED. |
    | 15 | `test_deleteFCNTUnderFCNT` | DELETE the nested &lt;FCNT&gt; -> expects DELETED. |
    | 16 | `test_deleteFCNT` | DELETE the &lt;FCNT&gt; -> expects DELETED. |
    | 17 | `test_createGenericInterworking` | CREATE a Generic Interworking Specialization &lt;FCNT&gt; (`m2m:gis`) with `cnd`, `gisn` -> expects CREATED. |
    | 18 | `test_createGenericInterworkingWrong` | CREATE a `m2m:gis` &lt;FCNT&gt; missing the mandatory `gisn` attribute -> expects BAD_REQUEST. |
    | 19 | `test_createGenericInterworkingWrong2` | CREATE a `m2m:gis` &lt;FCNT&gt; with an unknown attribute -> expects BAD_REQUEST. |
    | 20 | `test_createGenericInterworkingOperationInstance` | CREATE a `m2m:gio` (Generic Interworking Operation) &lt;FCNT&gt; under the `gis` resource with `gion`/`gios` -> expects CREATED. |
    | 21 | `test_createGenericInterworkingOperationInstance2` | CREATE another `m2m:gio` under `gis` with `gion`, `giip` (list), `gios` -> expects CREATED. |
    | 22 | `test_deleteGenericInterworking` | DELETE the `gis` resource (and its `gio` children) -> expects DELETED. |
    | 23 | `test_createFCNTWithCreatorWrong` | CREATE a `m2m:gis` &lt;FCNT&gt; with `cr` explicitly set to a wrong non-null value -> expects BAD_REQUEST. |
    | 24 | `test_createFCNTWithCreator` | CREATE a `m2m:gis` &lt;FCNT&gt; with `cr` set to `None` -> expects CREATED, checks `cr` auto-set to originator; RETRIEVE it -> expects OK, checks `cr` persists. |

??? note "`testFCNT_FCI.py` — Combined FlexContainer/FlexContainerInstance (&lt;FCNT&gt;/&lt;FCI&gt;) functionality and notifications (30 tests)"

    | # | Test Method | Requests Performed |
    |---|---|---|
    | 1 | `test_createFCNTwithMniMissingFciedFail` | CREATE a &lt;FCNT&gt; with `mni` set but `fcied` (FlexContainerInstance enabled) omitted -> expects BAD_REQUEST. |
    | 2 | `test_createFCNTwithMbsMissingFciedFail` | CREATE a &lt;FCNT&gt; with `mbs` set but `fcied` omitted -> expects BAD_REQUEST. |
    | 3 | `test_createFCNTwithMiaMissingFciedFail` | CREATE a &lt;FCNT&gt; with `mia` set but `fcied` omitted -> expects BAD_REQUEST. |
    | 4 | `test_createFCNTwithMni0Fail` | CREATE a &lt;FCNT&gt; with `fcied=False` and `mni=0` -> expects BAD_REQUEST. |
    | 5 | `test_createFCNTwithMbs0Fail` | CREATE a &lt;FCNT&gt; with `fcied=False` and `mbs=0` -> expects BAD_REQUEST. |
    | 6 | `test_createFCNTwithMia0Fail` | CREATE a &lt;FCNT&gt; with `fcied=False` and `mia=0` -> expects BAD_REQUEST. |
    | 7 | `test_createFCNTwithMniFciedFalse` | CREATE a &lt;FCNT&gt; with `fcied=False` and `mni=10` -> expects CREATED; DELETE it -> expects DELETED. |
    | 8 | `test_createFCNTwithMniFciedTrue` | CREATE a &lt;FCNT&gt; with `fcied=True` and `mni=10` -> expects CREATED (kept for subsequent tests). |
    | 9 | `test_attributesFCNT` | RETRIEVE the &lt;FCNT&gt; -> expects OK; checks `ty`, `pi`, `rn`, timestamps, `cr` absent, custom attributes (`cnd`, `curT0`, `unit`, `minVe`, `maxVe`, `steVe`), `st=0`, `fcied=True`, `mni=10`, `cni=1`, `cbs`&gt;0. |
    | 10 | `test_retrieveLatestFCI` | RETRIEVE the &lt;FCNT&gt;'s `la` (latest FlexContainerInstance) -> expects OK; checks `ty=FCI`, `st=0`, `org` matches originator, `cs`, and the snapshotted custom attribute values. |
    | 11 | `test_updateFCNT` | UPDATE the &lt;FCNT&gt; adding `tarTe` and changing `curT0` -> expects UPDATED; RETRIEVE it -> expects OK, checks the new values, `st=1`, `cni=2` (new FCI snapshot created), `cbs`&gt;0. |
    | 12 | `test_retrieveFCNTLatest` | RETRIEVE `la` -> expects OK; checks it reflects the updated `curT0=17.0`. |
    | 13 | `test_retrieveFCNTOldest` | RETRIEVE `ol` -> expects OK; checks it still reflects the original `curT0=23.0`. |
    | 14 | `test_updateFCNTMniReduce` | UPDATE the &lt;FCNT&gt; setting `mni=1` -> expects UPDATED; RETRIEVE it -> expects OK, checks `mni=1`, `cni=1`; RETRIEVE `la` and `ol` -> both expect OK and both now reference the same single remaining FCI. |
    | 15 | `test_updateLBL` | RETRIEVE the &lt;FCNT&gt; to record `cni`/`cbs`/`st`; UPDATE setting `lbl` -> expects UPDATED, checks `lbl`; RETRIEVE `la` -> expects OK, checks `lbl` and `st` incremented; RETRIEVE the &lt;FCNT&gt; again -> expects OK, checks `cni`/`cbs` increased (a new FCI snapshot was created for the label change). |
    | 16 | `test_updateLOC` | RETRIEVE the &lt;FCNT&gt; to record state; UPDATE setting `loc` (location) -> expects UPDATED; RETRIEVE `la` -> expects OK, checks `loc` and `st` incremented; RETRIEVE the &lt;FCNT&gt; again -> expects OK, checks `cni`/`cbs` increased. |
    | 17 | `test_updateNothing` | RETRIEVE the &lt;FCNT&gt; to record state; UPDATE with an empty body -> expects UPDATED; checks `cni`/`cbs` unchanged but `st` still incremented. |
    | 18 | `test_updateMNInoFCICreated` | RETRIEVE the &lt;FCNT&gt; to record `cni`; UPDATE `mni` (increase) -> expects UPDATED; checks `cni` is unchanged (changing `mni` alone doesn't create a new FCI snapshot). |
    | 19 | `test_createFCIFail` | CREATE a &lt;FCI&gt; directly (manually) under the &lt;FCNT&gt; -> expects OPERATION_NOT_ALLOWED. |
    | 20 | `test_updateFCIFail` | RETRIEVE the latest &lt;FCI&gt; -> expects OK; UPDATE it directly -> expects OPERATION_NOT_ALLOWED. |
    | 21 | `test_updateFCNTMniNull` | UPDATE the &lt;FCNT&gt; setting `mni=None` (remove restriction) -> expects UPDATED; RETRIEVE it -> expects OK, checks `mni` absent but `cni` still present; RETRIEVE `la`/`ol` -> both expect OK (history preserved). |
    | 22 | `test_updateFCNTMbsSmallFail` | RETRIEVE the &lt;FCNT&gt; to get its current `cs`; UPDATE `mbs` to `cs-1` (too small to hold current content) -> expects BAD_REQUEST. |
    | 23 | `test_updateFCNTMbsLarger` | RETRIEVE to get current `cs`; UPDATE `mbs` to `cs+1` -> expects UPDATED, checks `mbs` and `cni=1`; RETRIEVE `la` -> expects OK. |
    | 24 | `test_updateFCNTFciedFalse` | UPDATE the &lt;FCNT&gt; setting `mni=10`, removing `mbs`, and changing `curT0` -> expects UPDATED, checks `cni=2`, `mbs` absent, `mni=10`; UPDATE again setting `fcied=False` -> expects UPDATED, checks `mni`/`cni=1` (history collapsed to one) and `cbs` equals current `cs`; RETRIEVE `la`/`ol` -> both expect OK. |
    | 25 | `test_updateFCNTMniWithoutFciedFail` | UPDATE the &lt;FCNT&gt; (now with `fcied=False`) setting `mni=10` again -> expects BAD_REQUEST, checks `fcied`/`mni` absent in the error response; RETRIEVE `la`/`ol` -> both expect NOT_FOUND. |
    | 26 | `test_updateFCNTFciedRemoved` | UPDATE the &lt;FCNT&gt; setting `fcied=None` (remove the feature entirely) -> expects UPDATED, checks `mbs`/`mni`/`mia`/`cni`/`cbs` all absent; RETRIEVE `la` -> expects NOT_FOUND; RETRIEVE a (mistyped) `old` path -> expects NOT_FOUND. |
    | 27 | `test_updateFCNTFciedFalsePreviousNull` | UPDATE the &lt;FCNT&gt; setting `fcied=None` -> expects UPDATED, checks history attributes absent; RETRIEVE `la` -> expects NOT_FOUND; UPDATE again setting `fcied=False` -> expects UPDATED, checks `cni=1`/`cbs` matches `cs`; RETRIEVE `la`/`ol` -> both expect OK. |
    | 28 | `test_createFCNTFciedFalseEmptyFCNT` | CREATE a new &lt;FCNT&gt; (`cod:aiQSr`, no custom attributes) with `fcied=False` -> expects CREATED, checks `cni=1`/`cbs=0`; RETRIEVE `la`/`ol` -> both expect OK; DELETE the &lt;FCNT&gt; -> expects DELETED. |
    | 29 | `test_updateFCNTFciedFalseEmptyFCNT` | CREATE the same empty &lt;FCNT&gt; (`fcied=False`) -> expects CREATED; UPDATE setting `fcied=True` -> expects UPDATED, checks `cni=1`/`cbs=0` unchanged; RETRIEVE `la`/`ol` -> both expect OK; DELETE the &lt;FCNT&gt; -> expects DELETED. |
    | 30 | `test_deleteFCNT` | DELETE the main &lt;FCNT&gt; -> expects DELETED. |

??? note "`testTS.py` — TimeSeries (&lt;TS&gt;) resource functionality (33 tests)"

    | # | Test Method | Requests Performed |
    |---|---|---|
    | 1 | `test_createTS` | CREATE a &lt;TS&gt; under the &lt;AE&gt; with `pei`, `mdd=True`, `mdt`, `mdn`, `cnf` -> expects CREATED. |
    | 2 | `test_attributesTS` | RETRIEVE the &lt;TS&gt; -> expects OK; checks `ty`, `pi`, `cni=0`, `cbs=0`, `cnf`, `pei`, `peid` (auto-derived as `pei/2`), `mdd`, `mdn`, `mdlt` absent, `mdc=0`, `mdt`. |
    | 3 | `test_createTSunderTS` | CREATE a &lt;TS&gt; under the existing &lt;TS&gt; (invalid parent) -> expects INVALID_CHILD_RESOURCE_TYPE. |
    | 4 | `test_updateTSmni` | UPDATE the &lt;TS&gt; setting `mni=10` -> expects UPDATED; checks `mni`. |
    | 5 | `test_updateTSmbs` | UPDATE the &lt;TS&gt; setting `mbs=1000` -> expects UPDATED; checks `mbs`. |
    | 6 | `test_updateTSpeiFail` | UPDATE the &lt;TS&gt; attempting to change `pei` while monitoring is active -> expects BAD_REQUEST. |
    | 7 | `test_updateTSpeidFail` | UPDATE the &lt;TS&gt; attempting to change `peid` directly -> expects BAD_REQUEST. |
    | 8 | `test_updateTSmddTrue` | UPDATE the &lt;TS&gt; setting `mdd=True` (already true) -> expects UPDATED. |
    | 9 | `test_updateTSmddTrueAndMdtFail` | UPDATE the &lt;TS&gt; with `mdn` while `mdd` would conflict -> expects BAD_REQUEST. |
    | 10 | `test_updateTSmddTrueAndMdnFail` | UPDATE the &lt;TS&gt; setting `mdn` together with active monitoring in an invalid combination -> expects BAD_REQUEST. |
    | 11 | `test_updateTSmddTrueAndMdnNoneFail` | UPDATE the &lt;TS&gt; attempting to remove `mdn` (`None`) while monitoring is active -> expects BAD_REQUEST. |
    | 12 | `test_updateTSmddTrueAndPeiFail` | UPDATE the &lt;TS&gt; attempting to change `pei` while monitoring is active -> expects BAD_REQUEST. |
    | 13 | `test_updateTSmddTrueAndPeidFail` | UPDATE the &lt;TS&gt; attempting to change `pei` (mislabeled as peid test) while monitoring is active -> expects BAD_REQUEST. |
    | 14 | `test_updateTSmddFalse` | UPDATE the &lt;TS&gt; setting `mdd=False` -> expects UPDATED (monitoring disabled). |
    | 15 | `test_updateTSmddWithMdtFail` | UPDATE the &lt;TS&gt; setting `mdd=False` together with `mdt` -> expects BAD_REQUEST (mdt only valid while monitoring enabled). |
    | 16 | `test_updateTSmddWithMdnFail` | UPDATE the &lt;TS&gt; setting `mdd=False` together with `mdn` -> expects BAD_REQUEST. |
    | 17 | `test_updateTSmddWithPeiFail` | UPDATE the &lt;TS&gt; setting `mdd=False` together with `pei` -> expects BAD_REQUEST. |
    | 18 | `test_updateTSmddWithPeidFail` | UPDATE the &lt;TS&gt; setting `mdd=False` together with `peid` -> expects BAD_REQUEST. |
    | 19 | `test_updateTSmddWithWrongPeiPeidFail` | UPDATE the &lt;TS&gt; setting `pei` and `peid` to the same (invalid) value -> expects BAD_REQUEST. |
    | 20 | `test_updateTSmdn` | UPDATE the &lt;TS&gt; setting `mdn=5` -> expects UPDATED; checks `mdn`. |
    | 21 | `test_updateTSmdc` | UPDATE the &lt;TS&gt; attempting to directly set the read-only `mdc` (missing-data count) -> expects BAD_REQUEST. |
    | 22 | `test_updateTSmdlt` | UPDATE the &lt;TS&gt; attempting to directly set the read-only `mdlt` (missing-data list) -> expects BAD_REQUEST. |
    | 23 | `test_updateTScnf` | UPDATE the &lt;TS&gt; attempting to change `cnf` to an invalid value -> expects BAD_REQUEST. |
    | 24 | `test_updateTSremoveMdn` | UPDATE the &lt;TS&gt; setting `mdn=None` (remove) -> expects UPDATED; checks `mdn`/`mdlt` absent but `mdc` still present. |
    | 25 | `test_deleteTS` | DELETE the &lt;TS&gt; -> expects DELETED. |
    | 26 | `test_createTSnoMdd` | CREATE a &lt;TS&gt; without specifying `mdd` -> expects CREATED; checks `mdd` defaults to `False`. |
    | 27 | `test_updateTSMddwrong` | UPDATE a &lt;TS&gt; (created without monitoring support) setting `mdd=True` -> expects BAD_REQUEST. |
    | 28 | `test_createTSwithPeid` | CREATE a &lt;TS&gt; with `pei=1000` and `peid=500` (exactly half) -> expects CREATED. |
    | 29 | `test_createTSwithPeidWrong` | CREATE a &lt;TS&gt; with `peid` greater than `pei/2` -> expects BAD_REQUEST. |
    | 30 | `test_createTSwithCnfWrong` | CREATE a &lt;TS&gt; with an invalid `cnf` value -> expects BAD_REQUEST. |
    | 31 | `test_createTSwithMissingMdtFail` | CREATE a &lt;TS&gt; with `mdd=True` but no `mdt` -> expects BAD_REQUEST. |
    | 32 | `test_MddMdltMdcHandling` | CREATE a &lt;TS&gt; with just `mdt` (no explicit `mdd`) -> expects CREATED, checks `mdd`/`mdc=0`/`mdlt` absent; UPDATE toggling `mdd` False -> True -> False in sequence -> each expects UPDATED, checking `mdc`/`mdlt` persist correctly across toggles. |
    | 33 | `test_updatePeiCheckPeid` | CREATE a &lt;TS&gt; with no `pei` -> expects CREATED, checks `peid` absent; UPDATE setting `pei=1000` -> expects UPDATED, checks `peid` auto-derived to 500 and `mdc=0`/`mdlt` absent. |

??? note "`testTS_TSI.py` — Combined TimeSeries/TimeSeriesInstance (&lt;TS&gt;/&lt;TSI&gt;) functionality (25 tests)"

    | # | Test Method | Requests Performed |
    |---|---|---|
    | 1 | `test_addTSI` | CREATE a &lt;TSI&gt; under the &lt;TS&gt; with `dgt`/`con` -> expects CREATED, checks `con`, `dgt`, `cs=6`; RETRIEVE the &lt;TS&gt; -> expects OK, checks `cni=1`, `cbs=6`. |
    | 2 | `test_addMoreTSI` | CREATE 3 more &lt;TSI&gt;s in sequence -> each expects CREATED with matching `con`/`dgt`/`cs`; after each, RETRIEVE the &lt;TS&gt; -> expects OK, checks `cni`/`cbs` growing, then capping at `cni=3` once `mni=3` is reached (oldest evicted). |
    | 3 | `test_retrieveTSLa` | RETRIEVE the &lt;TS&gt;'s `la` (latest) -> expects OK, checks it's the most recently added &lt;TSI&gt; (`con='dValue'`). |
    | 4 | `test_retrieveTSOl` | RETRIEVE the &lt;TS&gt;'s `ol` (oldest) -> expects OK, checks it's the oldest still-retained &lt;TSI&gt; (`con='bValue'`, since the very first was evicted by `mni`). |
    | 5 | `test_changeTSMni` | UPDATE the &lt;TS&gt; setting `mni=1` -> expects UPDATED, checks `cni` is now 1, `cbs=6`; RETRIEVE `la` and `ol` -> both expect OK and both now point to the same single remaining &lt;TSI&gt;. |
    | 6 | `test_deleteTS` | DELETE the &lt;TS&gt; -> expects DELETED. |
    | 7 | `test_createTSwithMBS` | CREATE a &lt;TS&gt; with `mbs` (max byte size) set -> expects CREATED, checks `mbs`. |
    | 8 | `test_createTSIexactSize` | CREATE a &lt;TSI&gt; whose `con` size exactly equals `mbs` -> expects CREATED, checks `cs` equals `mbs`. |
    | 9 | `test_createTSItooLarge` | CREATE a &lt;TSI&gt; whose `con` size exceeds `mbs` by 1 -> expects NOT_ACCEPTABLE. |
    | 10 | `test_createTSIsForTSwithSize` | CREATE several &lt;TSI&gt;s that fill the &lt;TS&gt; up to `mbs` -> each expects CREATED; RETRIEVE `la` -> expects OK with the latest content; CREATE one more &lt;TSI&gt; (forcing eviction) -> expects CREATED; RETRIEVE `la` again -> expects OK with the new content; RETRIEVE the &lt;TS&gt; -> expects OK, checks `cni=3` and `cbs=mbs`. |
    | 11 | `test_createTSIwithoutDGT` | CREATE a &lt;TSI&gt; missing the mandatory `dgt` attribute -> expects BAD_REQUEST. |
    | 12 | `test_createTSIwithSameDGT` | CREATE a &lt;TSI&gt; with a given `dgt` -> expects CREATED; CREATE a 2nd &lt;TSI&gt; with the identical `dgt` -> expects CONFLICT. |
    | 13 | `test_createTSIwithSNR` | CREATE a &lt;TSI&gt; with `snr` (sequence number) set -> expects CREATED; RETRIEVE `la` -> expects OK, checks `snr` matches. |
    | 14 | `test_createTSwithMonitoring` | CREATE a &lt;TS&gt; with `pei`, `mdd=True`, `mdn`, `mdt` (missing-data monitoring enabled) -> expects CREATED; checks `pei`, `mdd`, `mdlt` (absent), `mdc=0`, `mdt`. |
    | 15 | `test_createTSIinPeriod` | Start monitoring; CREATE 3 &lt;TSI&gt;s spaced exactly at the periodic interval -> each expects CREATED; RETRIEVE the &lt;TS&gt; -> expects OK, checks `mdc=0` (no missing data detected); stop monitoring. |
    | 16 | `test_createTSIinPeriodDgtTooEarly` | Start monitoring; CREATE 4 &lt;TSI&gt;s whose `dgt` values are consistently earlier than the actual creation time -> each expects CREATED; RETRIEVE the &lt;TS&gt; -> expects OK, checks `mdc` &gt;= 3 (missing-data detected due to timing mismatch); stop monitoring. |
    | 17 | `test_createTSIinPeriodDgtTooLate` | Same pattern but `dgt` values are consistently later than actual creation -> RETRIEVE the &lt;TS&gt; -> expects OK, checks `mdc` &gt;= 3; stop monitoring. |
    | 18 | `test_createTSIinPeriodDgtWayTooEarly` | Start monitoring; CREATE 4 &lt;TSI&gt;s with `dgt` values minimally incrementing (effectively all 'too early') -> RETRIEVE the &lt;TS&gt; -> expects OK, checks `mdc` &gt;= 3; stop monitoring. |
    | 19 | `test_createTSInotInPeriod` | Helper-driven: CREATE 3 &lt;TSI&gt;s spaced further apart than the allowed period (missing data) -> RETRIEVE the &lt;TS&gt; -> expects OK, checks `mdc` reflects the expected missing-data count. |
    | 20 | `test_createTSInotInPeriodLarger` | Helper-driven with a larger expected-missing count (`maxMdn+1`) to test the `mdlt` list capping at `maxMdn` -> RETRIEVE the &lt;TS&gt; -> expects OK, checks `mdc` is capped/reflects the overflow correctly. |
    | 21 | `test_updateTSMddEnable` | UPDATE the &lt;TS&gt; re-enabling `mdd=True` -> expects UPDATED, checks `mdd`, `mdc=0`, `mdlt` absent; restarts monitoring via helper. |
    | 22 | `test_createMissingDataSubUnderTS` | CREATE a &lt;SUB&gt; under the &lt;TS&gt; with `enc.net=[8]` (missing-data event) and `enc.md` (duration/count thresholds) -> expects CREATED; checks a verification notification (`m2m:sgn/vrq`) was received referencing the new subscription. |
    | 23 | `test_createMissingDataForSub` | CREATE an initial &lt;TSI&gt; to start monitoring, then repeatedly CREATE further &lt;TSI&gt;s spaced at the period interval -> each expects CREATED; after each, checks a missing-data notification (`m2m:tsn` with `mdlt`/`mdc`) is received only when the configured missing-data threshold count is reached, and no notification otherwise. |
    | 24 | `test_deleteMissingDataSubUnderTS` | DELETE the missing-data &lt;SUB&gt; under the &lt;TS&gt; -> expects DELETED. |
    | 25 | `test_setMddToFalseAfterAWhile` | CREATE a &lt;TS&gt; with monitoring enabled (`pei`, `mdd=True`, `mdn`, `mdt`) -> expects CREATED, checks `mdc=0`; CREATE 5 &lt;TSI&gt;s with short sleeps (triggering missing-data detection) -> each expects CREATED; RETRIEVE the &lt;TS&gt; -> expects OK, checks `mdc`&gt;0 and `mdlt` non-empty; UPDATE setting `mdd=False` -> expects UPDATED, checks `cni`, `mdlt`, and `mdc` are preserved unchanged after disabling monitoring. |

??? note "`testTSB.py` — TimeSyncBeacon (&lt;TSB&gt;) resource functionality (8 tests)"

    | # | Test Method | Requests Performed |
    |---|---|---|
    | 1 | `test_createTSB` | CREATE a &lt;TSB&gt; under the &lt;AE&gt; with `bcnc=PERIODIC` and a notification target (`bcnu`) -> expects CREATED; checks `ri` and `bcnc` in the response; DELETE the &lt;TSB&gt; -> expects DELETED. |
    | 2 | `test_createTSBEmptyNuFail` | CREATE a &lt;TSB&gt; with an empty `bcnu` list -> expects BAD_REQUEST. |
    | 3 | `test_createTSBBcniFail` | CREATE a &lt;TSB&gt; with `bcnc=LOSS_OF_SYNCHRONIZATION` but also an (invalid for that mode) `bcni` interval set -> expects BAD_REQUEST. |
    | 4 | `test_createTSBLOSNoBcnrFail` | CREATE a &lt;TSB&gt; with `bcnc=PERIODIC` together with a `bcnt` threshold (invalid combination) -> expects BAD_REQUEST. |
    | 5 | `test_createTSBBcntNoBcnrFail` | CREATE a &lt;TSB&gt; with `bcnc=LOSS_OF_SYNCHRONIZATION` and `bcnt` set but no `bcnr` (beacon receiver) -> expects BAD_REQUEST. |
    | 6 | `test_createTSBBcniDefault` | CREATE a &lt;TSB&gt; with `bcnc=PERIODIC` and no explicit `bcni` -> expects CREATED; checks `bcnt` is absent and a default `bcni` string is assigned; DELETE the &lt;TSB&gt; -> expects DELETED. |
    | 7 | `test_createTSBBcntDefault` | CREATE a &lt;TSB&gt; with `bcnc=LOSS_OF_SYNCHRONIZATION` and `bcnr` set but no explicit `bcnt` -> expects CREATED; checks `bcni` is absent and a default `bcnt` > 0 is assigned; DELETE the &lt;TSB&gt; -> expects DELETED. |
    | 8 | `test_createTSBPeriodic` | CREATE a &lt;TSB&gt; with `bcnc=PERIODIC` and an explicit `bcni` interval -> expects CREATED; checks `bcni`/`bcnc` in the response; waits for the periodic interval and checks a `m2m:tsbn` beacon notification was actually received with matching `tbr` (beacon resource) and a valid `ctm` timestamp; DELETE the &lt;TSB&gt; -> expects DELETED. |
