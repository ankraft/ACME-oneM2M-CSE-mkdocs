# Subscriptions & Notifications

*5 test modules, 202 test cases. [&larr; Back to overview](index.md)*

??? note "`testSUB.py` — Subscription (&lt;SUB&gt;) resource lifecycle and notification behavior (94 tests)"

    | # | Test Method | Requests Performed |
    |---|---|---|
    | 1 | `test_createSUB` | CREATE a &lt;SUB&gt; under the &lt;CNT&gt; monitoring `resourceUpdate`/`createDirectChild` with `su` set -> expects CREATED; checks the verification notification (`vrq`, `sur` matching the new sub's `ri`, `cr` matching the originator). |
    | 2 | `test_retrieveSUB` | RETRIEVE the &lt;SUB&gt; -> expects OK. |
    | 3 | `test_retrieveSUBWithWrongOriginator` | RETRIEVE the &lt;SUB&gt; with an unauthorized originator -> expects ORIGINATOR_HAS_NO_PRIVILEGE. |
    | 4 | `test_attributesSUB` | RETRIEVE the &lt;SUB&gt; -> expects OK; checks `ty`, `pi`, `rn`, timestamps, `cr` absent, `enc.net` (2 entries), `nu`, `nct=1`, `nse`/`nsi` absent. |
    | 5 | `test_createSUBWrong` | CREATE a &lt;SUB&gt; with `nu` pointing to an unreachable notification URL -> expects SUBSCRIPTION_VERIFICATION_INITIATION_FAILED; RETRIEVE it -> expects NOT_FOUND (rolled back). |
    | 6 | `test_updateSUB` | UPDATE the &lt;SUB&gt; setting `exc=5` -> expects UPDATED; checks `exc`. |
    | 7 | `test_updateSUBwithNu` | CREATE a &lt;SUB&gt; with `nu` set to the originator itself -> expects CREATED; UPDATE `nu` to the real notification server -> expects UPDATED, checks a new verification notification was sent; DELETE the &lt;SUB&gt; -> expects DELETED. |
    | 8 | `test_updateCNT` | UPDATE the &lt;CNT&gt; with `lbl`/`mni`/`mbs` -> expects UPDATED; checks the resulting notification contains the full updated &lt;CNT&gt; resource and no `cr`. |
    | 9 | `test_addCIN2CNT` | CREATE a &lt;CIN&gt; under the &lt;CNT&gt; -> expects CREATED; checks the resulting notification contains the full &lt;CIN&gt; resource and no `cr`. |
    | 10 | `test_removeCNT` | DELETE the &lt;CNT&gt; -> expects DELETED; checks a deletion notification (`sud`) was sent with no `cr`. |
    | 11 | `test_addCNTAgain` | CREATE the &lt;CNT&gt; again -> expects CREATED. |
    | 12 | `test_deleteSUBByUnknownOriginator` | DELETE the &lt;SUB&gt; with an unauthorized originator -> expects ORIGINATOR_HAS_NO_PRIVILEGE. |
    | 13 | `test_deleteSUBByAssignedOriginator` | DELETE the &lt;SUB&gt; with the correct originator -> expects DELETED; checks a deletion notification (`sud`, no `cr`). |
    | 14 | `test_createSUBModifedAttributesWrong` | CREATE a &lt;SUB&gt; with `nct=modifiedAttributes` combined with `net=createDirectChild` (invalid combination) -> expects BAD_REQUEST. |
    | 15 | `test_createSUBModifedAttributes` | CREATE a &lt;SUB&gt; with `nct=modifiedAttributes` and `net=resourceUpdate` -> expects CREATED, checks `nct`; checks the verification notification. |
    | 16 | `test_updateCNTModifiedAttributes` | UPDATE the &lt;CNT&gt; setting `lbl` -> expects UPDATED; checks the notification contains only the updated `lbl` (no `ty`) and no `cr`. |
    | 17 | `test_updateCNTSameModifiedAttributes` | UPDATE the &lt;CNT&gt; setting the same `lbl` value again -> expects UPDATED; checks the notification again contains only `lbl`. |
    | 18 | `test_createSUBRI` | CREATE a &lt;SUB&gt; with `nct=ri` (notify with resource ID only) -> expects CREATED, checks `nct`; checks the verification notification. |
    | 19 | `test_updateCNTRI` | UPDATE the &lt;CNT&gt;'s `lbl` -> expects UPDATED; checks the notification contains `m2m:uri` ending with the &lt;CNT&gt;'s `ri`, and no `cr`. |
    | 20 | `test_createSUBForBatchNotificationNumber` | CREATE a &lt;SUB&gt; with `bn.num` (batch notification count, no `su`) -> expects CREATED, checks `bn.num`/`bn.dur` (auto-set default); checks the verification notification. |
    | 21 | `test_updateCNTBatch` | UPDATE the &lt;CNT&gt; N times (matching the batch count) -> each expects UPDATED; checks a single aggregated `m2m:agn` notification containing all N updates in order. |
    | 22 | `test_deleteSUBForBatchReceiveRemainingNotifications` | UPDATE the &lt;CNT&gt; once more (one outstanding notification below the batch threshold) -> expects UPDATED; DELETE the &lt;SUB&gt; -> expects DELETED; checks the outstanding notification was still delivered in a final batch. |
    | 23 | `test_createSUBForBatchNotificationDuration` | CREATE a &lt;SUB&gt; with `bn.dur` (batch duration, no `su`) -> expects CREATED, checks `bn.dur`/`bn.num` absent; checks the verification notification. |
    | 24 | `test_updateCNTBatchDuration` | UPDATE the &lt;CNT&gt; N times -> each expects UPDATED; checks NO notification arrives before the duration elapses, but a single aggregated batch notification with all N updates arrives after. |
    | 25 | `test_deleteSUBForBatchNotificationDuration` | DELETE the &lt;SUB&gt; -> expects DELETED. |
    | 26 | `test_createSUBWithEncAtr` | CREATE a &lt;SUB&gt; with `enc.atr=['lbl']` (monitor only the `lbl` attribute) -> expects CREATED, checks `enc.atr`; checks the verification notification. |
    | 27 | `test_updateCNTWithEncAtrLbl` | UPDATE the &lt;CNT&gt;'s `lbl` (a monitored attribute) -> expects UPDATED; checks a notification with the updated `lbl` is received. |
    | 28 | `test_updateCNTWithEncAtrLblWrong` | UPDATE the &lt;CNT&gt;'s `mni` (a non-monitored attribute) -> expects UPDATED; checks NO notification is sent. |
    | 29 | `test_updateSUBWithEncRemoved` | UPDATE the &lt;SUB&gt; setting `enc=None` -> expects UPDATED; checks `enc` reverts to a default (`net` containing `resourceUpdate`) rather than being fully removed. |
    | 30 | `test_deleteSUBWithEncAtr` | DELETE the &lt;SUB&gt; -> expects DELETED. |
    | 31 | `test_createSUBBatchNotificationNumberWithLn` | CREATE a &lt;SUB&gt; with `bn.num` and `ln=True` (latest-notification-only batching) -> expects CREATED, checks `bn.num`/`ln`; checks the verification notification. |
    | 32 | `test_updateCNTBatchWithLn` | UPDATE the &lt;CNT&gt; N times -> each expects UPDATED; checks the resulting batch notification contains only 1 signal (the latest), and the `EC` (event category) header indicates 'latest'. |
    | 33 | `test_deleteSUBBatchNotificationNumberWithLn` | DELETE the &lt;SUB&gt; -> expects DELETED. |
    | 34 | `test_createSUBWithEncChty` | CREATE a &lt;SUB&gt; monitoring `createDirectChild` restricted to `chty=[CNT]` -> expects CREATED, checks `enc.chty`; checks the verification notification. |
    | 35 | `test_createCINWithEncChty` | CREATE a &lt;CIN&gt; under the &lt;CNT&gt; (not matching `chty`) -> expects CREATED; checks NO notification is sent. |
    | 36 | `test_createCNTWithEncChty` | CREATE a nested &lt;CNT&gt; (matching `chty`) -> expects CREATED; checks a notification with the new &lt;CNT&gt;'s `rn` is received; DELETE the nested &lt;CNT&gt; -> expects DELETED. |
    | 37 | `test_deleteSUBWithEncChty` | DELETE the &lt;SUB&gt; -> expects DELETED. |
    | 38 | `test_createAESUBwithOriginatorPOA` | CREATE a 2nd &lt;AE&gt; with `poa` set and `rr=True` (request reachable); CREATE a &lt;SUB&gt; under it with `nu` pointing to that AE's own originator -> both expect CREATED; checks NO verification request was sent directly (delivered via `poa` resolution instead, asynchronously). |
    | 39 | `test_createAESUBwithOriginatorPOANotReachable` | CREATE a 2nd &lt;AE&gt; with `poa` set but `rr=False` (not request reachable); CREATE a &lt;SUB&gt; with `nu` pointing to its originator -> both expect CREATED, checks no immediate verification; CREATE a &lt;CNT&gt; under it -> expects CREATED; checks NO notification is delivered (since the target isn't request-reachable). |
    | 40 | `test_createCNTwithOriginatorPOA` | CREATE a &lt;CNT&gt; under the request-reachable 2nd &lt;AE&gt; -> expects CREATED; checks a notification IS delivered via the AE's `poa`. |
    | 41 | `test_updateAECSZwithOriginatorPOA` | UPDATE the 2nd &lt;AE&gt; setting `csz=['application/cbor']` -> expects UPDATED; checks `csz`. |
    | 42 | `test_createCNTwithOriginatorPOACBOR` | CREATE another &lt;CNT&gt; under the 2nd &lt;AE&gt; (now CBOR-capable) -> expects CREATED; checks the notification is delivered with a CBOR `Content-Type` header. |
    | 43 | `test_deleteAEwithOriginatorPOA` | DELETE the 2nd &lt;AE&gt; (and its &lt;SUB&gt;) -> expects DELETED. |
    | 44 | `test_deleteSUBWithCreator` | CREATE a &lt;SUB&gt; with `cr=None` (auto-assigned) -> expects CREATED; DELETE it -> expects DELETED; checks the deletion notification's `sud` is true and `cr` matches the original creator. |
    | 45 | `test_deleteSUBWithoutCreator` | CREATE a &lt;SUB&gt; without `cr` -> expects CREATED; DELETE it -> expects DELETED; checks the deletion notification's `sud` is true and `cr` is absent. |
    | 46 | `test_createAESUBwithURIctCBOR` | CREATE a 2nd &lt;AE&gt; (no `poa`); CREATE a &lt;SUB&gt; with `nu` containing a direct URI with `ct=cbor` query param -> both expect CREATED; checks a verification request (`vrq`) was sent. |
    | 47 | `test_createCNTwithURIctCBOR` | CREATE a &lt;CNT&gt; under the 2nd &lt;AE&gt; -> expects CREATED; checks the notification is delivered with a CBOR `Content-Type` header via the direct URI. |
    | 48 | `test_deleteAEwithURIctCBOR` | DELETE the 2nd &lt;AE&gt; -> expects DELETED. |
    | 49 | `test_createSUBwithEXC` | CREATE a &lt;SUB&gt; with `exc=2` (expire after 2 notifications) -> expects CREATED; checks the verification notification. |
    | 50 | `test_createCNTforEXC` | CREATE a &lt;CNT&gt; -> expects CREATED, checks the resulting notification; RETRIEVE the &lt;SUB&gt; -> expects OK (1st notification consumed, still alive); CREATE a 2nd &lt;CNT&gt; -> expects CREATED; RETRIEVE the &lt;SUB&gt; again -> expects NOT_FOUND (expired after the 2nd notification). |
    | 51 | `test_createSUBwithUnknownPoa` | CREATE a &lt;SUB&gt; with `nu` pointing to an &lt;AE&gt; that has no `poa` -> expects SUBSCRIPTION_VERIFICATION_INITIATION_FAILED. |
    | 52 | `test_createSUBWithCreatorWrong` | CREATE a &lt;SUB&gt; with `cr` explicitly set to a wrong non-null value -> expects BAD_REQUEST. |
    | 53 | `test_createSUBWithCreatorNull` | CREATE a &lt;SUB&gt; with `cr=None` -> expects CREATED, checks `cr` auto-set to originator; checks the verification notification's `cr` matches; RETRIEVE the &lt;SUB&gt; -> expects OK, checks `cr` persists. |
    | 54 | `test_notifySUBWithCreatorNull` | CREATE a &lt;SUB&gt; with `cr=None` monitoring `resourceUpdate` -> expects CREATED, checks `cr`; checks the verification notification's `cr`; UPDATE the &lt;CNT&gt;'s `lbl` -> expects UPDATED; checks the resulting notification's `cr` matches the originator and `nev.net=resourceUpdate`. |
    | 55 | `test_createSUBForMissingDataFail` | CREATE a &lt;SUB&gt; under a &lt;CNT&gt; (non-TS parent) monitoring `reportOnGeneratedMissingDataPoints` -> expects BAD_REQUEST (missing-data only valid under &lt;TS&gt;). |
    | 56 | `test_createSUBForMissingDataFail2` | CREATE a &lt;TS&gt; -> expects CREATED; CREATE a &lt;SUB&gt; under it monitoring missing-data events but without the required `md` (missingData criteria) -> expects BAD_REQUEST. |
    | 57 | `test_createSUBForMissingData` | CREATE a &lt;TS&gt; -> expects CREATED; CREATE a &lt;SUB&gt; under it with `enc.md.num=5`/`dur=PT10S` -> expects CREATED; checks `enc.net` includes the missing-data event code and `enc.md.num`/`dur` match. |
    | 58 | `test_updateSUBForMissingData` | UPDATE the missing-data &lt;SUB&gt; setting `md.num=10`/`dur=PT20S` -> expects UPDATED; checks the updated values. |
    | 59 | `test_deleteSUBForMissingData` | DELETE the missing-data &lt;SUB&gt; -> expects DELETED. |
    | 60 | `test_createSUBForMissingDataWrongDataFail` | CREATE a &lt;TS&gt; -> expects CREATED; CREATE 5 separate &lt;SUB&gt;s each with a different malformed `md` (negative `num`, missing `dur`, missing `num`, `dur` as a plain number string, `dur` as an int) -> each expects BAD_REQUEST. |
    | 61 | `test_createSUBWithWrongCHTYFail` | CREATE a &lt;SUB&gt; under the &lt;AE&gt; with `enc.chty=[AE]` (an invalid child type for that parent) -> expects BAD_REQUEST. |
    | 62 | `test_updateSUBWithWrongCHTYFail` | CREATE a &lt;SUB&gt; with a valid `enc.chty=[CNT]` -> expects CREATED; UPDATE it to `enc.chty=[AE]` (invalid) -> expects BAD_REQUEST. |
    | 63 | `test_createSUBWithStructuredNu` | CREATE a &lt;SUB&gt; under the POA-enabled &lt;AE&gt; with `nu` given as a structured CSE-relative path (instead of a direct URI) -> expects CREATED; checks no verification notification is sent over HTTP (delivered via target resolution instead); DELETE the &lt;SUB&gt; -> expects DELETED. |
    | 64 | `test_createSubBlockingUpdateWrongNUFail` | CREATE a &lt;SUB&gt; for `net=blockingUpdate` with an unreachable `nu` -> expects SUBSCRIPTION_VERIFICATION_INITIATION_FAILED. |
    | 65 | `test_createSubBlockingUpdateMultipleNUFail` | CREATE a &lt;SUB&gt; for `net=blockingUpdate` with 2 entries in `nu` (must be exactly 1) -> expects BAD_REQUEST. |
    | 66 | `test_createSubBlockingUpdateDisallowedAttributeFail` | CREATE a &lt;SUB&gt; for `net=blockingUpdate` combined with `bn` (batch notification, disallowed for blocking) -> expects BAD_REQUEST. |
    | 67 | `test_createSubBlockingUpdateDisallowedENCAttributeFail` | CREATE a &lt;SUB&gt; for `net=blockingUpdate` combined with `enc.sza` (disallowed attribute for blocking) -> expects BAD_REQUEST. |
    | 68 | `test_createSubBlockingUpdateDisallowedNCTvaluel` | CREATE a &lt;SUB&gt; for `net=blockingUpdate` with `nct=allAttributes` (disallowed value for blocking) -> expects BAD_REQUEST. |
    | 69 | `test_createSubBlockingUpdate` | CREATE a valid blocking-update &lt;SUB&gt; under the POA &lt;AE&gt; with `enc.atr=['lbl']` and `nct=modifiedAttributes` -> expects CREATED. |
    | 70 | `test_updateSubBlockingUpdateFail` | UPDATE the blocking-update &lt;SUB&gt; re-setting `enc.net=[blockingUpdate]` (not allowed via UPDATE) -> expects BAD_REQUEST. |
    | 71 | `test_doBlockingUpdate` | UPDATE the POA &lt;AE&gt;'s `lbl` (triggering the blocking subscription) -> expects UPDATED; checks the resulting notification has `nev.net=blockingUpdate` and contains the updated `lbl`. |
    | 72 | `test_doBlockingUpdateAttributeCondition` | UPDATE the POA &lt;AE&gt;'s `apn` (an attribute not in the blocking `enc.atr` set) -> expects UPDATED; checks NO notification is sent (condition not matched). |
    | 73 | `test_doBlockingUpdateNegativeNotificationResponse` | Configure the next notification response to be a rejection; UPDATE the POA &lt;AE&gt;'s `lbl` -> expects OPERATION_DENIED_BY_REMOTE_ENTITY (the blocking notification target rejected the update). |
    | 74 | `test_deleteSubBlockingUpdate` | DELETE the blocking-update &lt;SUB&gt; -> expects DELETED. |
    | 75 | `test_createSUBForNotificationStats` | CREATE a &lt;SUB&gt; under the POA &lt;AE&gt; with `nse=True` -> expects CREATED, checks `nse=True`/`nsi` absent; checks the verification notification. |
    | 76 | `test_updateAECheckStats` | UPDATE the POA &lt;AE&gt;'s `lbl` -> expects UPDATED; RETRIEVE the &lt;SUB&gt; -> expects OK, checks `nsi[0]` has `rqs=1`/`rsr=1`/`noec=1`. |
    | 77 | `test_updateSUBNSEFalse` | UPDATE the &lt;SUB&gt; setting `nse=False` -> expects UPDATED; checks `nsi` stats are preserved unchanged. |
    | 78 | `test_updateSUBNSETrue` | UPDATE the &lt;SUB&gt; setting `nse=True` -> expects UPDATED; checks `nsi` is reset to empty. |
    | 79 | `test_updateSUBNSETrueAgain` | UPDATE the POA &lt;AE&gt;'s `lbl` again (generating new stats) -> expects UPDATED; RETRIEVE the &lt;SUB&gt; -> expects OK, checks `nsi` has exactly 1 fresh entry with `rqs=1`/`rsr=1`. |
    | 80 | `test_updateSUBcountBatchNotifications` | UPDATE the &lt;SUB&gt; enabling `nse=True` and `bn.num` -> expects UPDATED, checks `nsi` empty; UPDATE the POA &lt;AE&gt; N times to trigger a batch -> each expects UPDATED; checks the aggregated notification has N signals; RETRIEVE the &lt;SUB&gt; -> expects OK, checks `nsi[0]` reflects `rqs=5`/`rsr=5`/`noec=5`. |
    | 81 | `test_updateSUBDeleteNSE` | UPDATE the &lt;SUB&gt; setting `nse=None` (remove stats) -> expects UPDATED; checks both `nse`/`nsi` absent. |
    | 82 | `test_createSUBForOperationMonitor` | CREATE a &lt;SUB&gt; with `enc.om` (operationMonitor: `ops=UPDATE`, `org`=originator) -> expects CREATED; checks `enc.om` matches; DELETE the &lt;SUB&gt; -> expects DELETED. |
    | 83 | `test_updateSUBForOperationMonitor` | CREATE a &lt;SUB&gt; with `enc.net=resourceUpdate` -> expects CREATED; UPDATE it replacing `enc` with `om` (removing `net`) -> expects UPDATED, checks `net` absent and `om` present; DELETE the &lt;SUB&gt; -> expects DELETED. |
    | 84 | `test_createSUBForOperationMonitorFail` | CREATE a &lt;SUB&gt; with both `enc.net` and `enc.om` set simultaneously (mutually exclusive) -> expects BAD_REQUEST. |
    | 85 | `test_updateSUBForOperationMonitorFail` | CREATE a &lt;SUB&gt; with `enc.net` -> expects CREATED; UPDATE it adding `enc.om` while keeping `net` (both set together) -> expects BAD_REQUEST; DELETE the &lt;SUB&gt; -> expects DELETED. |
    | 86 | `test_createSUBForOperationMonitorEmptyOmFail` | CREATE a &lt;SUB&gt; with `enc.om` containing one valid entry and one empty `{}` entry -> expects BAD_REQUEST. |
    | 87 | `test_correctOperationMonitor` | CREATE a &lt;SUB&gt; with `enc.om` monitoring UPDATE by the originator -> expects CREATED; UPDATE the &lt;AE&gt;'s `lbl` with that originator -> expects UPDATED; checks the notification's `nev.om.ops`/`org` match; DELETE the &lt;SUB&gt; -> expects DELETED. |
    | 88 | `test_correctOperationMonitorMultiple` | CREATE a &lt;SUB&gt; with `enc.om` containing 2 entries (CREATE and UPDATE) for the same originator -> expects CREATED; UPDATE the &lt;AE&gt;'s `lbl` -> expects UPDATED; checks the notification matches the UPDATE entry; DELETE the &lt;SUB&gt; -> expects DELETED. |
    | 89 | `test_correctOperationMonitorRetrieve` | CREATE a &lt;SUB&gt; with `enc.om` monitoring RETRIEVE -> expects CREATED; RETRIEVE the &lt;AE&gt; with that originator -> expects OK; checks the notification's `nev.om.ops=2`(RETRIEVE)/`org` match; DELETE the &lt;SUB&gt; -> expects DELETED. |
    | 90 | `test_OperationMonitorOtherOriginatorFail` | CREATE a &lt;SUB&gt; with `enc.om.org` set to a different originator than the one performing the operation -> expects CREATED; UPDATE the &lt;AE&gt; with the actual (non-matching) originator -> expects UPDATED; checks NO notification is sent (originator mismatch); DELETE the &lt;SUB&gt; -> expects DELETED. |
    | 91 | `test_testSUBwithEnableEventNotificationOriginator` | CREATE a &lt;SUB&gt; with `eeno=True` (notify with the event's actual originator, not the sub's) -> expects CREATED, checks `eeno`; UPDATE the &lt;AE&gt; using the CSE admin originator -> expects UPDATED; checks the notification's `cr` matches the admin originator (not the SUB's creator); DELETE the &lt;SUB&gt; -> expects DELETED. |
    | 92 | `test_testSUBwithEnableEventNotificationOriginatorAndCreator` | CREATE a &lt;SUB&gt; with `eeno=True` and `cr=None` -> expects CREATED, checks `eeno`/`cr`; UPDATE the &lt;AE&gt; using the CSE admin originator -> expects UPDATED; checks the notification's `cr` is the SUB's own creator (since `eeno` falls back when a creator is explicitly recorded) -- DELETE the &lt;SUB&gt; -> expects DELETED. |
    | 93 | `test_createSUBnoNCTwrongNETFail` | CREATE a &lt;SUB&gt; with `enc.net` containing `resourceUpdate` and `blockingUpdate` together (conflicting NET values) and `nse=True` -> expects BAD_REQUEST. |
    | 94 | `test_deleteNuAttributeFail` | CREATE a &lt;SUB&gt; with `nu` set -> expects CREATED; UPDATE it setting `nu=None` (removing the mandatory attribute) -> expects BAD_REQUEST; DELETE the &lt;SUB&gt; -> expects DELETED. |

??? note "`testCRS.py` — CrossResourceSubscription (&lt;CRS&gt;) functionality and notifications (71 tests)"

    | # | Test Method | Requests Performed |
    |---|---|---|
    | 1 | `test_createCRSmissingRratSratFail` | CREATE a &lt;CRS&gt; with neither `rrat` nor `srat` -> expects BAD_REQUEST. |
    | 2 | `test_createCRSmissingNuFail` | CREATE a &lt;CRS&gt; with `rrat` set but `nu` missing -> expects BAD_REQUEST. |
    | 3 | `test_createCRSmissingTwtFail` | CREATE a &lt;CRS&gt; with `twt` (time window type) missing -> expects BAD_REQUEST. |
    | 4 | `test_createCRSwrongTwtFail` | CREATE a &lt;CRS&gt; with an invalid `twt` value (`99`) -> expects BAD_REQUEST. |
    | 5 | `test_createCRSmissingTwsFail` | CREATE a &lt;CRS&gt; with `tws` (time window size) missing -> expects BAD_REQUEST. |
    | 6 | `test_createCRSemptyEncsFail` | CREATE a &lt;CRS&gt; with an empty `encs` -> expects BAD_REQUEST. |
    | 7 | `test_createCRSWrongNumberEncsFail` | CREATE a &lt;CRS&gt; with `encs.enc` containing 3 entries (more than the 2 `rrat` targets support) -> expects BAD_REQUEST. |
    | 8 | `test_createCRSwithRratsFail` | CREATE a &lt;CRS&gt; explicitly setting the read-only `rrats` attribute -> expects BAD_REQUEST. |
    | 9 | `test_createCRSwithRrat` | CREATE a &lt;CRS&gt; with `rrat` referencing 2 &lt;CNT&gt;s, periodic window (`twt=1`), one `encs` entry -> expects CREATED; checks 2 auto-created `rrats` subscriptions exist (one per CNT, verified via discovery) and a 3rd unrelated CNT has none. |
    | 10 | `test_createCRSwithSingleRratAndNSI` | CREATE a &lt;CRS&gt; with a single `rrat`, `nse=True` (notification stats enabled) -> expects CREATED; checks exactly 1 auto-created subscription. |
    | 11 | `test_createCRSwithRratSlidingStatsEnabled` | CREATE a &lt;CRS&gt; with `rrat` (2 targets), sliding window (`twt=2`), `nse=True` -> expects CREATED; checks `nse=True`, `nsi` absent, and 2 auto-created subscriptions. |
    | 12 | `test_updateCRSwithNewRratFail` | UPDATE the &lt;CRS&gt; adding a 3rd target to `rrat` -> expects BAD_REQUEST (rrat is immutable after creation). |
    | 13 | `test_updateCRSwithrratsFail` | UPDATE the &lt;CRS&gt; setting the read-only `rrats` -> expects BAD_REQUEST. |
    | 14 | `test_updateCRSwithNecFail` | UPDATE the &lt;CRS&gt; setting `nec` -> expects BAD_REQUEST (not updatable). |
    | 15 | `test_updateCRSwithEncsFail` | UPDATE the &lt;CRS&gt; setting `encs` -> expects BAD_REQUEST (immutable after creation). |
    | 16 | `test_updateCRSwithDeletedEncsFail` | UPDATE the &lt;CRS&gt; setting `encs=None` while `rrat` is present -> expects BAD_REQUEST. |
    | 17 | `test_createCRSwithRratAndEt` | CREATE a &lt;CRS&gt; with `rrat` and an explicit `et` -> expects CREATED, checks `et`; RETRIEVE both auto-created subscriptions -> each expects OK with the same `et` propagated. |
    | 18 | `test_createCRSwithRratWrongTarget` | CREATE a &lt;CRS&gt; with `rrat` referencing a non-existent resource ID -> expects CROSS_RESOURCE_OPERATION_FAILURE. |
    | 19 | `test_deleteCRSwithRrat` | DELETE the &lt;CRS&gt; -> expects DELETED; checks the auto-created subscriptions for all 3 containers are now gone. |
    | 20 | `test_createCRSwithSratNonSubFail` | CREATE a &lt;CRS&gt; with `srat` pointing to a non-&lt;SUB&gt; resource -> expects BAD_REQUEST; checks no subscription exists. |
    | 21 | `test_createSubscriptions` | CREATE 2 standalone &lt;SUB&gt;s (one under each of the first 2 &lt;CNT&gt;s) for use in subsequent `srat` tests -> both expect CREATED. |
    | 22 | `test_createCRSwithSrat` | CREATE a &lt;CRS&gt; with `srat` referencing the first &lt;SUB&gt; -> expects CREATED; checks both containers' subscriptions exist (one explicit, one auto-linked); RETRIEVE the first &lt;SUB&gt; directly -> expects OK, checks the CRS's SP-relative `ri` appears in both `nu` and `acrs`. |
    | 23 | `test_updateCRSwithNewSratFail` | UPDATE the &lt;CRS&gt; adding a 2nd `srat` entry -> expects BAD_REQUEST (immutable). |
    | 24 | `test_deleteCRSwithSrat` | DELETE the &lt;CRS&gt; -> expects DELETED; checks both &lt;SUB&gt;s no longer reference the CRS in `nu`/`acrs`, and the 3rd container's subscription is absent. |
    | 25 | `test_deleteRratSubscription` | DELETE one of the 2 `rrat`-derived auto-subscriptions directly -> expects DELETED; checks the other auto-subscription and the &lt;CRS&gt; itself are also cascaded-deleted (NOT_FOUND). |
    | 26 | `test_createSrat2Subscriptions` | CREATE a &lt;CRS&gt; with `srat` referencing both standalone &lt;SUB&gt;s -> expects CREATED. |
    | 27 | `test_deleteSratSubscription` | DELETE the first referenced &lt;SUB&gt; -> expects DELETED; checks the 2nd &lt;SUB&gt; still exists but the &lt;CRS&gt; is cascaded-deleted (NOT_FOUND). |
    | 28 | `test_deleteSubscriptions` | Cleanup: DELETE either remaining standalone &lt;SUB&gt; if present -> each expects DELETED. |
    | 29 | `test_updateSubAcrs` | UPDATE one &lt;SUB&gt; setting `acrs=[]` (detach from CRS) -> expects UPDATED, checks the CRS's `ri` removed from `acrs`; checks the &lt;CRS&gt; itself is now deleted and the 2nd &lt;SUB&gt;'s `acrs` is also cleared. |
    | 30 | `test_createSingleNotificationNoNotification` | CREATE a single &lt;CIN&gt; under one monitored &lt;CNT&gt; -> expects CREATED; waits past the time window; checks NO CRS notification was sent (only one of the rrat targets fired). |
    | 31 | `test_createTwoSingleNotificationNoNotifications` | CREATE a &lt;CIN&gt; under the first &lt;CNT&gt;, wait past the window (no notification); CREATE a &lt;CIN&gt; under the second &lt;CNT&gt; in a separate window, wait again -> checks NO notification either time (events didn't co-occur within a single window). |
    | 32 | `test_createTwoNotificationOneNotification` | Restart the CRS's window timer via UPDATE -> expects UPDATED; CREATE a &lt;CIN&gt; under each of the 2 monitored &lt;CNT&gt;s within the same window -> both expect CREATED; checks exactly one aggregated CRS notification is received, with `sgn/sur` matching the CRS's `ri`. |
    | 33 | `test_updateCRSPeriodicWindowSize` | UPDATE the &lt;CRS&gt;'s `tws` to double the periodic window size -> expects UPDATED; CREATE 2 &lt;CIN&gt;s under both monitored containers -> both expect CREATED; checks no notification at ~60% of the window, but a notification has arrived by the full window's end. |
    | 34 | `test_enableSlidingWindow` | UPDATE the &lt;CRS&gt; setting `twt=SLIDINGWINDOW` and a doubled `tws` -> expects UPDATED. |
    | 35 | `test_updateCRSSlidingWindowSize` | UPDATE the sliding `tws`; CREATE a &lt;CIN&gt; under the first container -> expects CREATED, checks no notification yet; after a short additional delay, CREATE a &lt;CIN&gt; under the second container (resetting/extending the sliding window) -> expects CREATED; checks a notification eventually arrives. |
    | 36 | `test_retrieveCRSwithNSENSINone` | RETRIEVE the &lt;CRS&gt; -> expects OK; checks `nse=True` and `nsi` absent (no notifications yet). |
    | 37 | `test_updateCRSwithDeletedNse` | UPDATE the &lt;CRS&gt; setting `nse=None` (disable stats) -> expects UPDATED; checks both `nse` and `nsi` absent. |
    | 38 | `test_updateCRSwithDeletedNsi` | UPDATE the &lt;CRS&gt; setting the read-only `nsi=None` directly -> expects BAD_REQUEST. |
    | 39 | `test_testEmptyNsi` | RETRIEVE the &lt;CRS&gt; -> expects OK; checks `nse=True` and `nsi` still absent (no notification activity yet). |
    | 40 | `test_updateCRSwithEnableNSE` | UPDATE the &lt;CRS&gt; setting `nse=True` -> expects UPDATED; checks `nse` present and `nsi` absent (reset). |
    | 41 | `test_testNonEmptyNsi` | RETRIEVE the &lt;CRS&gt; -> expects OK; checks `nsi` now has exactly 1 stats entry with `tg` (target), `rqs=1`, `rsr=1`, `noec=1` matching the prior notification activity. |
    | 42 | `test_updateCRSwithNseFalse` | UPDATE the &lt;CRS&gt; setting `nse=False` -> expects UPDATED; checks `nse=False` but `nsi` still retains its 1 entry (stats preserved when disabling). |
    | 43 | `test_updateCRSwithNseTrue` | UPDATE the &lt;CRS&gt; setting `nse=True` again -> expects UPDATED; checks `nse=True` and `nsi` reset to empty. |
    | 44 | `test_createCRSwithExpiration` | CREATE a &lt;CRS&gt; with `exc=2` (expiration counter) and sliding window -> expects CREATED; trigger 2 rounds of notification (one &lt;CIN&gt; per monitored CNT, twice) -> each CREATE expects CREATED; checks the &lt;CRS&gt; is auto-deleted (NOT_FOUND) once the counter is exhausted. |
    | 45 | `test_testCRSwithSu` | CREATE a &lt;CRS&gt; with `su` (subscriber URI) set -> expects CREATED, checks `su`; DELETE the &lt;CRS&gt; -> expects DELETED; checks a deletion notification (`sgn/sud`) was sent to the subscriber. |
    | 46 | `test_createCorrectEEM12345TWT1` | Loop over `eem` values 1-5 with `twt=1` (periodic, all valid combinations) -> each CREATE expects CREATED; each is immediately DELETEd -> expects DELETED. |
    | 47 | `test_createCorrectEEM125TWT2` | Loop over `eem` values 1, 2, 5 with `twt=2` (sliding, valid combinations) -> each CREATE expects CREATED; each DELETEd -> expects DELETED. |
    | 48 | `test_createWrongEEM34TWT2` | Loop over `eem` values 3, 4 with `twt=2` (sliding, invalid combinations — those EEMs require periodic) -> each CREATE expects BAD_REQUEST. |
    | 49 | `test_updateCorrectEEM12345TWT1` | CREATE a periodic &lt;CRS&gt; -> expects CREATED; loop UPDATE through `eem` 1-5 -> each expects UPDATED; DELETE -> expects DELETED. |
    | 50 | `test_updateCorrectEEM125TWT2` | CREATE a sliding &lt;CRS&gt; -> expects CREATED; loop UPDATE through `eem` 1, 2, 5 -> each expects UPDATED; DELETE -> expects DELETED. |
    | 51 | `test_updateWrongEEM34TWT2` | CREATE a sliding &lt;CRS&gt; -> expects CREATED; loop UPDATE through `eem` 3, 4 (invalid for sliding) -> each expects BAD_REQUEST; DELETE -> expects DELETED. |
    | 52 | `test_createCRSPeriodicAllSomeEventsPresentAll` | CREATE a periodic &lt;CRS&gt; with `eem=2` (ALL_OR_SOME_EVENTS_PRESENT) -> expects CREATED; CREATE a &lt;CIN&gt; under both monitored &lt;CNT&gt;s (all events present) -> both expect CREATED; checks a notification IS received; DELETE -> expects DELETED. |
    | 53 | `test_createCRSPeriodicAllSomeEventsPresentSome` | Same `eem=2` setup but only 1 of 2 &lt;CNT&gt;s gets a &lt;CIN&gt; (some events) -> checks a notification IS still received; DELETE -> expects DELETED. |
    | 54 | `test_createCRSPeriodicAllSomeEventsPresentNone` | Same `eem=2` setup with no &lt;CIN&gt;s created -> checks NO notification; DELETE -> expects DELETED. |
    | 55 | `test_createCRSPeriodicAllSomeEventsMissingAll` | CREATE a periodic &lt;CRS&gt; with `eem=3` (ALL_OR_SOME_EVENTS_MISSING) -> expects CREATED; CREATE &lt;CIN&gt;s under both containers (no events missing) -> checks NO notification; DELETE -> expects DELETED. |
    | 56 | `test_createCRSPeriodicAllSomeEventsMissingSome` | Same `eem=3` setup with only 1 of 2 containers getting a &lt;CIN&gt; (some events missing) -> checks a notification IS received; DELETE -> expects DELETED. |
    | 57 | `test_createCRSPeriodicAllSomeEventsMissingNone` | Same `eem=3` setup with no &lt;CIN&gt;s (all events missing) -> checks a notification IS received; DELETE -> expects DELETED. |
    | 58 | `test_createCRSPeriodicAllEventsMissingAll` | CREATE a periodic &lt;CRS&gt; with `eem=4` (ALL_EVENTS_MISSING) -> expects CREATED; CREATE &lt;CIN&gt;s under both containers -> checks NO notification (events were present, not all missing); DELETE -> expects DELETED. |
    | 59 | `test_createCRSPeriodicAllEventsMissingSome` | Same `eem=4` setup with only 1 of 2 events present -> checks NO notification (not ALL missing); DELETE -> expects DELETED. |
    | 60 | `test_createCRSPeriodicAllEventsMissingNone` | Same `eem=4` setup with no events at all -> checks a notification IS received (all events truly missing); DELETE -> expects DELETED. |
    | 61 | `test_createCRSSlidingAllSomeEventsPresentAll` | Same as the periodic ALL_OR_SOME_EVENTS_PRESENT-All test but with `twt=2` (sliding window) -> checks a notification IS received; DELETE -> expects DELETED. |
    | 62 | `test_createCRSSlidingAllSomeEventsPresentSome` | Sliding-window variant with only 1 of 2 events -> checks a notification IS received; DELETE -> expects DELETED. |
    | 63 | `test_createCRSSlidingAllSomeEventsPresentNone` | Sliding-window variant with no events -> checks NO notification; DELETE -> expects DELETED. |
    | 64 | `test_createCRSPeriodicSomeEventsMissingAll` | CREATE a periodic &lt;CRS&gt; with `eem=5` (SOME_EVENTS_MISSING) -> expects CREATED; CREATE &lt;CIN&gt;s under both containers -> checks NO notification (no events missing); DELETE -> expects DELETED. |
    | 65 | `test_createCRSPeriodicSomeEventsMissingSome` | Same `eem=5` setup with only 1 of 2 events present -> checks a notification IS received (some events missing); DELETE -> expects DELETED. |
    | 66 | `test_createCRSPeriodicSomeEventsMissingNone` | Same `eem=5` setup with no events -> checks a notification IS received; DELETE -> expects DELETED. |
    | 67 | `test_createCRSSlidingSomeEventsMissingAll` | Sliding-window `eem=5` variant with all events present -> checks NO notification; DELETE -> expects DELETED. |
    | 68 | `test_createCRSSlidingSomeEventsMissingSome` | Sliding-window `eem=5` variant with some events present -> checks a notification IS received; DELETE -> expects DELETED. |
    | 69 | `test_createCRSSlidingSomeEventsMissingNone` | Sliding-window `eem=5` variant with no events -> checks a notification IS received; DELETE -> expects DELETED. |
    | 70 | `test_deleteCRSWithCreator` | CREATE a &lt;CRS&gt; with `cr=None` (creator auto-assigned) and `su` set -> expects CREATED; DELETE it -> expects DELETED; checks the deletion notification's `sgn/sud` is true and `sgn/cr` matches the original creator. |
    | 71 | `test_deleteCRSWithoutCreator` | CREATE a &lt;CRS&gt; without setting `cr` -> expects CREATED; DELETE it -> expects DELETED; checks the deletion notification's `sgn/sud` is true and `sgn/cr` is absent. |

??? note "`testNTP.py` — NotificationTargetPolicy resource functionality (8 tests)"

    | # | Test Method | Requests Performed |
    |---|---|---|
    | 1 | `test_createNTPWithAdminCreatorFail` | CREATE an &lt;NTP&gt; under the &lt;CSEBase&gt; using the CSE admin originator as `cr` (creator) -> expects BAD_REQUEST. |
    | 2 | `test_createNTP` | CREATE an &lt;NTP&gt; under the &lt;CSEBase&gt; with the AE's originator -> expects CREATED. |
    | 3 | `test_retrieveNTP` | RETRIEVE the &lt;NTP&gt; -> expects OK. |
    | 4 | `test_updateNTP` | UPDATE the &lt;NTP&gt; setting `lbl` -> expects UPDATED; checks the returned `lbl` matches. |
    | 5 | `test_deleteNTP` | DELETE the &lt;NTP&gt; -> expects DELETED. |
    | 6 | `test_createNTPwithSameCreatorAndLabelFail` | CREATE a 2nd &lt;NTP&gt; with the same creator/label combination as an existing default policy -> expects BAD_REQUEST. |
    | 7 | `test_updateNTPwithSameCreatorAndLabelFail` | CREATE an &lt;NTP&gt; with a distinct label -> expects CREATED; UPDATE it to use a label that collides with another existing policy for the same creator -> expects BAD_REQUEST; DELETE the &lt;NTP&gt; (cleanup, result not checked). |
    | 8 | `test_deleteSystemDefaultNTPFail` | DELETE the CSE's built-in default &lt;NTP&gt; (`defaultNTP`) -> expects BAD_REQUEST (system default policy cannot be removed). |

??? note "`testNTPR.py` — NotificationTargetMgmtPolicyRef resource functionality (6 tests)"

    | # | Test Method | Requests Performed |
    |---|---|---|
    | 1 | `test_createNTPR` | CREATE an &lt;NTPR&gt; under the &lt;SUB&gt; (created in setUp), with `ntu` set to the notification server -> expects CREATED. |
    | 2 | `test_retrieveNTPR` | RETRIEVE the &lt;NTPR&gt; -> expects OK. |
    | 3 | `test_updateNTPR` | UPDATE the &lt;NTPR&gt; setting `lbl` -> expects UPDATED; checks the returned `lbl` matches. |
    | 4 | `test_deleteNTPR` | DELETE the &lt;NTPR&gt; -> expects DELETED. |
    | 5 | `test_createNTPRDuplicateNotificationTargetURIFail` | CREATE a 1st &lt;NTPR&gt; with a given `ntu` -> expects CREATED; CREATE a 2nd &lt;NTPR&gt; under the same &lt;SUB&gt; with the same `ntu` -> expects CONFLICT (duplicate notificationTargetURI); DELETE the 1st &lt;NTPR&gt; -> expects DELETED. |
    | 6 | `test_updateNTPRDuplicateNotificationTargetURIFail` | CREATE a 1st &lt;NTPR&gt; with one `ntu` -> expects CREATED; CREATE a 2nd &lt;NTPR&gt; with a different `ntu` -> expects CREATED; UPDATE the 2nd &lt;NTPR&gt; to use the same `ntu` as the 1st -> expects CONFLICT; DELETE both &lt;NTPR&gt;s -> each expects DELETED. |

??? note "`testNTSR.py` — NotificationTargetSelfReference resource functionality and handling (23 tests)"

    | # | Test Method | Requests Performed |
    |---|---|---|
    | 1 | `test_retrieveNTSRFail` | CREATE a &lt;SUB&gt; -> expects CREATED; RETRIEVE its `ntsr` (notificationTargetSelfReference virtual resource) -> expects OPERATION_NOT_ALLOWED; DELETE the &lt;SUB&gt; -> expects DELETED. |
    | 2 | `test_createNTSRFail` | CREATE a &lt;SUB&gt; -> expects CREATED; attempt to CREATE an `ntsr` resource under it directly -> expects something other than CREATED; DELETE the &lt;SUB&gt; -> expects DELETED. |
    | 3 | `test_updateNTSRFail` | CREATE a &lt;SUB&gt; -> expects CREATED; UPDATE its `ntsr` -> expects OPERATION_NOT_ALLOWED; DELETE the &lt;SUB&gt; -> expects DELETED. |
    | 4 | `test_unsubscribeUsingSystemDefault` | CREATE a &lt;SUB&gt; (notification target = 2nd AE) -> expects CREATED; DELETE the &lt;SUB&gt;'s `ntsr` with the 2nd AE's originator, relying on the system default NTP (assumed reject) -> expects ORIGINATOR_HAS_NO_PRIVILEGE; DELETE the &lt;SUB&gt; -> expects DELETED. |
    | 5 | `test_unsubscribeUsingCreatorDefaultAccept` | CREATE a &lt;SUB&gt; -> expects CREATED; CREATE a creator-default &lt;NTP&gt; with `acn=1` (accept) -> expects CREATED; DELETE the `ntsr` via the 2nd AE -> expects DELETED; RETRIEVE the &lt;SUB&gt; -> expects OK, checks `nu` is now empty (unsubscribed); cleanup DELETEs -> expect DELETED. |
    | 6 | `test_unsubscribeUsingCreatorDefaultReject` | Same setup but the &lt;NTP&gt; has `acn=2` (reject) -> DELETE the `ntsr` -> expects ORIGINATOR_HAS_NO_PRIVILEGE; RETRIEVE the &lt;SUB&gt; -> expects OK, checks `nu` still has 1 entry (not unsubscribed); cleanup DELETEs -> expect DELETED. |
    | 7 | `test_unsubscribeUsingCreatorDefaultSeekAuthorizationAccept` | Same setup with `acn=3` (seek authorization) and the notification server set to answer positively -> DELETE the `ntsr` -> expects DELETED; RETRIEVE the &lt;SUB&gt; -> expects OK, checks `nu` is empty; checks an authorization-request notification (`m2m:sgn` with `tra` set) was received; cleanup -> expect DELETED. |
    | 8 | `test_unsubscribeUsingCreatorDefaultSeekAuthorizationReject` | Same `acn=3` setup but the notification server answers negatively -> DELETE the `ntsr` -> expects ORIGINATOR_HAS_NO_PRIVILEGE; RETRIEVE the &lt;SUB&gt; -> expects OK, checks `nu` still has 1 entry; checks the authorization-request notification was received; cleanup -> expect DELETED. |
    | 9 | `test_unsubscribeUsingCreatorDefaultInformReject` | Same setup with `acn=4` (inform & reject) -> DELETE the `ntsr` -> expects ORIGINATOR_HAS_NO_PRIVILEGE; RETRIEVE the &lt;SUB&gt; -> expects OK, checks `nu` still has 1 entry; checks an informational notification (`m2m:sgn` with `trr` set) was received; cleanup -> expect DELETED. |
    | 10 | `test_unsubscribeUsingNTPRMatching` | CREATE a &lt;SUB&gt;, a non-default &lt;NTP&gt; (`acn=1` accept), and an &lt;NTPR&gt; under the &lt;SUB&gt; referencing that NTP with a matching `ntu` -> each expects CREATED; DELETE the `ntsr` -> expects DELETED; RETRIEVE the &lt;SUB&gt; -> expects OK, checks `nu` empty; cleanup -> expect DELETED. |
    | 11 | `test_unsubscribeUsingNTPRNoNPI` | Same but the &lt;NTPR&gt; has no `npi` (policy reference) -> DELETE the `ntsr` falls back to the system default (reject) -> expects ORIGINATOR_HAS_NO_PRIVILEGE; RETRIEVE the &lt;SUB&gt; -> expects OK, checks `nu` still has 1 entry; cleanup -> expect DELETED. |
    | 12 | `test_unsubscribeUsingNTPRNotMatchingNTU` | Same but the &lt;NTPR&gt;'s `ntu` doesn't match the unsubscribing originator -> DELETE the `ntsr` falls back to system default (reject) -> expects ORIGINATOR_HAS_NO_PRIVILEGE; RETRIEVE the &lt;SUB&gt; -> expects OK, checks `nu` still has 1 entry; cleanup -> expect DELETED. |
    | 13 | `test_unsubscribeWrongNTPReference` | CREATE a &lt;SUB&gt; and an &lt;NTPR&gt; whose `npi` points to a non-existent &lt;NTP&gt; -> expects CREATED; DELETE the `ntsr` falls back to system default (reject) -> expects ORIGINATOR_HAS_NO_PRIVILEGE; RETRIEVE the &lt;SUB&gt; -> expects OK, checks `nu` still has 1 entry; cleanup -> expect DELETED. |
    | 14 | `test_unsubscribeWithPDRNoRules` | CREATE &lt;SUB&gt;, &lt;NTP&gt; (accept), &lt;NTPR&gt;, and a &lt;PDR&gt; with no decision rules -> each expects CREATED; DELETE the `ntsr` -> expects DELETED (no rules = always allowed); RETRIEVE the &lt;SUB&gt; -> expects OK, checks `nu` empty; cleanup -> expect DELETED. |
    | 15 | `test_unsubscribeWithPDREmptyRules` | Same but the &lt;PDR&gt; has an empty `tod` schedule list -> DELETE the `ntsr` -> expects DELETED; RETRIEVE the &lt;SUB&gt; -> expects OK, checks `nu` empty; cleanup -> expect DELETED. |
    | 16 | `test_unsubscribeWithPDRScheduleSimple` | Same but the &lt;PDR&gt; has a single always-matching schedule (`* * * * * * *`) -> DELETE the `ntsr` -> expects DELETED; RETRIEVE the &lt;SUB&gt; -> expects OK, checks `nu` empty; cleanup -> expect DELETED. |
    | 17 | `test_unsubscribeWithPDRScheduleSimpleWrongTime` | Same but the &lt;PDR&gt;'s single schedule doesn't currently match -> DELETE the `ntsr` -> expects ORIGINATOR_HAS_NO_PRIVILEGE; RETRIEVE the &lt;SUB&gt; -> expects OK, checks `nu` still has 1 entry; cleanup -> expect DELETED. |
    | 18 | `test_unsubscribeWithPDRScheduleORSingle` | Same but the &lt;PDR&gt; has 2 schedules combined with OR (`drr=2`), one matching, one not -> DELETE the `ntsr` -> expects DELETED (OR satisfied); RETRIEVE the &lt;SUB&gt; -> expects OK, checks `nu` empty; cleanup -> expect DELETED. |
    | 19 | `test_unsubscribeWithPDRScheduleANDSingle` | Same but combined with AND (`drr=1`), one matching, one not -> DELETE the `ntsr` -> expects ORIGINATOR_HAS_NO_PRIVILEGE (AND not satisfied); RETRIEVE the &lt;SUB&gt; -> expects OK, checks `nu` still has 1 entry; cleanup -> expect DELETED. |
    | 20 | `test_unsubscribeWithPDRScheduleORDoubleANDAccept` | Same but 2 separate &lt;PDR&gt;s (combined via NTP's `rrs=1`/AND across policies), each internally OR of 2 schedules and both individually true -> DELETE the `ntsr` -> expects DELETED; RETRIEVE the &lt;SUB&gt; -> expects OK, checks `nu` empty; cleanup -> expect DELETED. |
    | 21 | `test_unsubscribeWithPDRScheduleORDoubleANDReject` | Same 2-policy AND-across-policies setup but only one of the two &lt;PDR&gt;s evaluates true -> DELETE the `ntsr` -> expects ORIGINATOR_HAS_NO_PRIVILEGE; RETRIEVE the &lt;SUB&gt; -> expects OK, checks `nu` still has 1 entry; cleanup -> expect DELETED. |
    | 22 | `test_unsubscribeWithPDRScheduleANDDoubleORAccept` | Same but NTP's `rrs=2`/OR across the 2 &lt;PDR&gt;s, each internally AND of 2 schedules, and only one policy is fully true -> DELETE the `ntsr` -> expects DELETED (OR across policies satisfied); RETRIEVE the &lt;SUB&gt; -> expects OK, checks `nu` empty; cleanup -> expect DELETED. |
    | 23 | `test_unsubscribeWithPDRScheduleANDDoubleORReject` | Same OR-across-policies setup but both &lt;PDR&gt;s evaluate false (AND of mismatched schedules) -> DELETE the `ntsr` -> expects ORIGINATOR_HAS_NO_PRIVILEGE; RETRIEVE the &lt;SUB&gt; -> expects OK, checks `nu` still has 1 entry; cleanup -> expect DELETED. |
