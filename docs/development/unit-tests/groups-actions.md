# Groups & Actions

*3 test modules, 80 test cases. [&larr; Back to overview](index.md)*

??? note "`testGRP.py` — Group (&lt;GRP&gt;) resource lifecycle and fanOut behavior (23 tests)"

    | # | Test Method | Requests Performed |
    |---|---|---|
    | 1 | `test_createGRP` | CREATE a &lt;GRP&gt; under the &lt;AE&gt; with `mt=MIXED`, `mnm=10`, and `mid` referencing 2 &lt;CNT&gt;s -> expects CREATED. |
    | 2 | `test_retrieveGRP` | RETRIEVE the &lt;GRP&gt; -> expects OK. |
    | 3 | `test_retrieveGRPWithWrongOriginator` | RETRIEVE the &lt;GRP&gt; with an unauthorized originator -> expects ORIGINATOR_HAS_NO_PRIVILEGE. |
    | 4 | `test_attributesGRP` | RETRIEVE the &lt;GRP&gt; -> expects OK; checks `ty`, `pi`, `rn`, `ct`, `lt`, `et`, `cr` (absent), `mt`, `mnm=10`, `cnm=2`, `mid` (list of 2), and `st` (absent). |
    | 5 | `test_updateGRP` | UPDATE the &lt;GRP&gt; setting `mnm=15` -> expects UPDATED; checks the returned `mnm`. |
    | 6 | `test_updateGRPwithCNT` | UPDATE the &lt;GRP&gt; with a body typed as `m2m:cnt` instead of `m2m:grp` -> expects something other than UPDATED. |
    | 7 | `test_addCNTtoGRP` | RETRIEVE the &lt;GRP&gt; -> expects OK, checks `cnm=2`; CREATE a 3rd &lt;CNT&gt; -> expects CREATED; UPDATE the &lt;GRP&gt;'s `mid` to add it -> expects UPDATED, checks `cnm` is now 3. |
    | 8 | `test_addCINviaFOPT` | CREATE a &lt;CIN&gt; via the group's fanOutPoint (`/fopt`) targeting all 3 member &lt;CNT&gt;s -> expects OK on the aggregated request, checks 3 individual CREATED responses; RETRIEVE each created &lt;CIN&gt; by its returned `ri` -> each expects OK; RETRIEVE each &lt;CNT&gt;'s `la` (latest) directly -> each expects OK with the matching `con`. |
    | 9 | `test_retrieveLAviaFOPT` | RETRIEVE the group's `/fopt/la` (latest of all 3 member &lt;CNT&gt;s) -> expects OK on the aggregated request, checks 3 individual OK responses, each RETRIEVE-verified directly by `ri`. |
    | 10 | `test_updateCNTviaFOPT` | UPDATE all 3 member &lt;CNT&gt;s via `/fopt` setting `lbl` -> expects OK on the aggregated request, checks 3 individual UPDATED responses, each verified by a direct RETRIEVE that `lbl` was applied. |
    | 11 | `test_addExistingCNTtoGRP` | RETRIEVE the &lt;GRP&gt; -> expects OK; UPDATE `mid` to add a duplicate of an already-member &lt;CNT&gt; -> expects UPDATED, checks `cnm` is unchanged (no duplicate counted). |
    | 12 | `test_deleteCNTviaFOPT` | DELETE all 3 member &lt;CNT&gt;s via `/fopt` -> expects OK on the aggregated request, checks 3 individual DELETED responses. |
    | 13 | `test_deleteGRPByUnknownOriginator` | DELETE the &lt;GRP&gt; with an unauthorized originator -> expects ORIGINATOR_HAS_NO_PRIVILEGE. |
    | 14 | `test_deleteGRPByAssignedOriginator` | DELETE the &lt;GRP&gt; with the correct originator -> expects DELETED. |
    | 15 | `test_createGRP2` | Re-create the 2 member &lt;CNT&gt;s; CREATE a new &lt;GRP&gt; with `mnm=2` and `mid` referencing both -> expects CREATED. |
    | 16 | `test_addTooManyCNTToGRP2` | CREATE a 3rd &lt;CNT&gt; -> expects CREATED; UPDATE the &lt;GRP&gt; (with `mnm=2`) to add it as a 3rd member -> expects MAX_NUMBER_OF_MEMBER_EXCEEDED. |
    | 17 | `test_addDeleteContainerCheckMID` | RETRIEVE the &lt;GRP&gt; -> expects OK, checks `cnm=2`; CREATE a 4th &lt;CNT&gt; -> expects CREATED; UPDATE the &lt;GRP&gt;'s `mid` to add it -> expects UPDATED, checks `cnm=3`; DELETE that &lt;CNT&gt; directly -> expects DELETED; RETRIEVE the &lt;GRP&gt; again -> expects OK, checks `cnm` reverted to 2 and the deleted CNT's `ri` no longer in `mid`. |
    | 18 | `test_attributesGRP2` | RETRIEVE the &lt;GRP&gt; -> expects OK; checks attributes (`ty`, `pi`, `rn`, timestamps, `mt`, `mnm=2`, `cnm=2`, `mid` length 2, `st` absent) confirming the failed over-limit UPDATE didn't change membership. |
    | 19 | `test_createGRPWithCreatorWrong` | CREATE a &lt;GRP&gt; with `cr` explicitly set to a wrong non-null value -> expects BAD_REQUEST. |
    | 20 | `test_createGRPWithCreator` | CREATE a &lt;GRP&gt; with `cr` set to `None` -> expects CREATED, checks `cr` auto-set to originator; RETRIEVE it -> expects OK, checks `cr` persists. |
    | 21 | `test_createCNTviaFopt` | CREATE a &lt;CNT&gt; under each of the group's 2 member &lt;CNT&gt;s via `/fopt` -> expects OK on the aggregated request, checks 2 individual responses. |
    | 22 | `test_retrieveCNTviaFopt` | RETRIEVE the just-created nested &lt;CNT&gt; under each member via `/fopt/container` -> expects OK on the aggregated request, checks 2 individual OK responses with matching `rn`, `ty`, and `cni=0`. |
    | 23 | `test_createCNTCNTviaFopt` | CREATE another nested &lt;CNT&gt; one level deeper under each member's `container` via `/fopt/container` -> expects OK on the aggregated request, checks 2 individual responses. |

??? note "`testACTR.py` — Action (&lt;ACTR&gt;) resource lifecycle, subject/action/dependency evaluation, and triggering (39 tests)"

    | # | Test Method | Requests Performed |
    |---|---|---|
    | 1 | `test_createACTRWrongORCFail` | CREATE an &lt;ACTR&gt; under the &lt;AE&gt; with `orc` (originator's resource) set to an invalid value (`'todo'`) -> expects BAD_REQUEST. |
    | 2 | `test_createACTRNoAccessORCFail` | CREATE an &lt;ACTR&gt; with `orc` pointing to a &lt;CNT&gt; the originator has no access to -> expects BAD_REQUEST. |
    | 3 | `test_createACTRNoAccessSRIFail` | CREATE an &lt;ACTR&gt; with `sri` (subject resource) pointing to a &lt;CNT&gt; with no access, while `orc` is valid -> expects BAD_REQUEST. |
    | 4 | `test_createACTRWrongEVCAttributeFail` | CREATE an &lt;ACTR&gt; with an unknown attribute inside `evc` -> expects BAD_REQUEST. |
    | 5 | `test_createACTRInvalidAPVFail` | CREATE an &lt;ACTR&gt; with an empty `apv` (action to be performed) -> expects BAD_REQUEST. |
    | 6 | `test_createACTRInvalidFromFail` | CREATE an &lt;ACTR&gt; with `apv.fr` set to an invalid originator -> expects BAD_REQUEST. |
    | 7 | `test_createACTRInvalidEVMRangeFail` | CREATE an &lt;ACTR&gt; with `evm` set to an out-of-range value (`99`) -> expects BAD_REQUEST. |
    | 8 | `test_createACTRInvalidEVMandECP1Fail` | CREATE an &lt;ACTR&gt; with `evm=off` together with `ecp` (evaluation check period, invalid for that mode) -> expects BAD_REQUEST. |
    | 9 | `test_createACTRInvalidEVMandECP2Fail` | CREATE an &lt;ACTR&gt; with `evm=once` together with `ecp` (invalid for that mode) -> expects BAD_REQUEST. |
    | 10 | `test_createACTRwithInvalidSBJTFail` | CREATE an &lt;ACTR&gt; with `evc.sbjt` set to an unknown subject attribute -> expects BAD_REQUEST. |
    | 11 | `test_createACTRwithInvalidTHLDFail` | CREATE an &lt;ACTR&gt; with `evc.thld` set to a negative number where a non-negative integer is required -> expects BAD_REQUEST. |
    | 12 | `test_createACTRwithInvalidOPTRFail` | CREATE an &lt;ACTR&gt; with `evc.optr` (operator, `lessThan`) incompatible with its `sbjt`/`thld` (a list) -> expects BAD_REQUEST. |
    | 13 | `test_createACTRnoSRI` | CREATE a valid &lt;ACTR&gt; with no `sri` -> expects CREATED; checks `evm`, `evc`, `orc`, `apv.op`/`fr`/`to`/`rqi` all match what was specified. |
    | 14 | `test_createACTRwithSRI` | CREATE a valid &lt;ACTR&gt; with `sri` set -> expects CREATED; checks all the same attributes plus `sri`. |
    | 15 | `test_createACTRwithSRIOnce` | CREATE a valid &lt;ACTR&gt; with `sri` and `evm=once` -> expects CREATED; checks all attributes. |
    | 16 | `test_createACTRwithSRIPeriodic` | CREATE a valid &lt;ACTR&gt; with `sri`, `evm=periodic`, and `ecp` -> expects CREATED; checks all attributes including `ecp`. |
    | 17 | `test_createACTRwithSRIContinuous` | CREATE a valid &lt;ACTR&gt; with `sri`, `evm=continuous`, and `ecp` -> expects CREATED; checks all attributes. |
    | 18 | `test_updateACTROnceECPFail` | UPDATE the once-mode &lt;ACTR&gt; setting `evm=once` together with `ecp` (invalid combination) -> expects BAD_REQUEST. |
    | 19 | `test_updateACTRContinuousWithOnceFail` | UPDATE the continuous-mode &lt;ACTR&gt; switching to `evm=once` -> expects BAD_REQUEST (existing `ecp` makes the combination invalid). |
    | 20 | `test_updateACTRonceWrongSRIFail` | UPDATE the once-mode &lt;ACTR&gt; setting `sri` to a non-existent resource -> expects BAD_REQUEST. |
    | 21 | `test_updateACTRonceWrongORCFail` | UPDATE the once-mode &lt;ACTR&gt; setting `orc` to a non-existent resource -> expects BAD_REQUEST. |
    | 22 | `test_updateACTRonceWrongSBJTFail` | UPDATE the once-mode &lt;ACTR&gt;'s `evc.sbjt` to an unknown attribute -> expects BAD_REQUEST. |
    | 23 | `test_updateACTRonceSRIWrongSBJTFail` | UPDATE the once-mode &lt;ACTR&gt; setting both a valid new `sri` and an invalid `evc.sbjt` together -> expects BAD_REQUEST. |
    | 24 | `test_updateACTRonceNewSRIOldWrongSBJTFail` | UPDATE the once-mode &lt;ACTR&gt; setting only a new `sri` (whose resource type doesn't support the existing `evc.sbjt`) -> expects BAD_REQUEST. |
    | 25 | `test_updateACTRonceNewEVCWrongTHLDFail` | UPDATE the once-mode &lt;ACTR&gt;'s `evc.thld` to a type-mismatched value (`cni` subject given a string) -> expects BAD_REQUEST. |
    | 26 | `test_updateACTRonceORCNullFail` | UPDATE the once-mode &lt;ACTR&gt; setting `orc=None` (removing it, not allowed) -> expects BAD_REQUEST. |
    | 27 | `test_updateACTRConceWithOnce` | UPDATE the once-mode &lt;ACTR&gt; re-setting `evm=once` (now valid alone) -> expects UPDATED; checks `evm`. |
    | 28 | `test_updateACTRConceWithPeriodic` | UPDATE the &lt;ACTR&gt; setting `evm=periodic` -> expects UPDATED; checks `evm`. |
    | 29 | `test_updateACTRConceWithContinuous` | UPDATE the &lt;ACTR&gt; setting `evm=continuous` -> expects UPDATED; checks `evm`. |
    | 30 | `test_updateACTRConceWithOff` | UPDATE the &lt;ACTR&gt; setting `evm=off` -> expects UPDATED; checks `evm`. |
    | 31 | `test_deleteACTRnoSRI` | DELETE the no-SRI &lt;ACTR&gt; -> expects DELETED. |
    | 32 | `test_deleteACTRwithSRI` | DELETE the SRI &lt;ACTR&gt; -> expects DELETED. |
    | 33 | `test_deleteACTRwithSRIOnce` | DELETE the SRI-once &lt;ACTR&gt; -> expects DELETED. |
    | 34 | `test_deleteACTRwithSRIPeriodic` | DELETE the SRI-periodic &lt;ACTR&gt; -> expects DELETED. |
    | 35 | `test_deleteACTRwithSRIContinuous` | DELETE the SRI-continuous &lt;ACTR&gt; -> expects DELETED. |
    | 36 | `test_testACTROnce` | CREATE a &lt;CNT&gt;; CREATE an &lt;ACTR&gt; under it in `evm=once` mode with `sbjt=cni`, targeting the &lt;AE&gt; with an UPDATE action (`apv`) -> each expects CREATED; CREATE a &lt;CIN&gt; (condition not yet met) -> checks no label applied; CREATE a 2nd &lt;CIN&gt; (condition met) -> checks the label IS applied via RETRIEVE; checks the &lt;ACTR&gt;'s `air` (action invocation result) reflects an UPDATED response with the label payload; DELETE the &lt;CNT&gt; -> expects DELETED. |
    | 37 | `test_testACTRParentOnce` | Same once-mode scenario but using the parent subject reference (no explicit `sri`, defaulting to the parent) -> same CREATE/CIN/label-check/cleanup flow as `test_testACTROnce`. |
    | 38 | `test_testACTRContinuous` | Same scenario but `evm=continuous` with `ecp=2` and explicit `sri` -> after the 2nd &lt;CIN&gt; the label is applied and verified via `air`; the label is removed; a 3rd &lt;CIN&gt; is created and checked that NO label is reapplied (continuous mode fires once per qualifying transition, not every match); DELETE the &lt;CNT&gt; -> expects DELETED. |
    | 39 | `test_testACTRPeriodic` | Same scenario but `evm=periodic` with a longer `ecp` -> CREATE a &lt;CIN&gt; -> checks label applied; remove label; CREATE a 2nd &lt;CIN&gt; within the same period -> checks NO label re-applied; sleep past the next period; CREATE a 3rd &lt;CIN&gt; -> checks the label IS applied again in the new period; DELETE the &lt;CNT&gt; -> expects DELETED. |

??? note "`testDEPR.py` — Dependency (&lt;DEPR&gt;) resource functionality (18 tests)"

    | # | Test Method | Requests Performed |
    |---|---|---|
    | 1 | `test_createDEPRUnderAE` | CREATE a &lt;DEPR&gt; directly under the &lt;AE&gt; (instead of under an &lt;ACTR&gt;) -> expects INVALID_CHILD_RESOURCE_TYPE. |
    | 2 | `test_createDEPRWrongRRIFail` | CREATE a &lt;DEPR&gt; under the &lt;ACTR&gt; with `rri` (referenced resource ID) pointing to a non-existent resource -> expects BAD_REQUEST. |
    | 3 | `test_createDEPRWrongSBJTFail` | CREATE a &lt;DEPR&gt; with `evc.sbjt` (evaluation subject attribute) set to an invalid/unknown attribute name -> expects BAD_REQUEST. |
    | 4 | `test_createDEPRWrongTHLDTypeFail` | CREATE a &lt;DEPR&gt; with `evc.thld` (threshold) of a type incompatible with the subject attribute (`cni`, a number, given as a string) -> expects BAD_REQUEST. |
    | 5 | `test_createDEPR` | CREATE a valid &lt;DEPR&gt; under the &lt;ACTR&gt; with `evc` (cbs &gt;= 1), `sfc=True`, and `rri` pointing to the &lt;CNT&gt; -> expects CREATED; DELETE it -> expects DELETED. |
    | 6 | `test_updateDEPRWrongTypeFail` | CREATE a &lt;DEPR&gt; -> expects CREATED; attempt to UPDATE the &lt;ACTR&gt; (wrong resource/type) with a `m2m:depr` body -> expects CONTENTS_UNACCEPTABLE; DELETE the &lt;DEPR&gt; -> expects DELETED. |
    | 7 | `test_updateDEPRsfc` | CREATE a &lt;DEPR&gt; -> expects CREATED; UPDATE it setting `sfc=False` -> expects UPDATED; DELETE it -> expects DELETED. |
    | 8 | `test_updateDEPRrri` | CREATE a &lt;DEPR&gt; -> expects CREATED; UPDATE its `rri` to point to the 2nd &lt;CNT&gt; -> expects UPDATED; DELETE it -> expects DELETED. |
    | 9 | `test_updateDEPRevc` | CREATE a &lt;DEPR&gt; with one `evc` -> expects CREATED; UPDATE `evc` to a different operator -> expects UPDATED; DELETE it -> expects DELETED. |
    | 10 | `test_updateACTRdepWrongFail` | UPDATE the &lt;ACTR&gt; setting `dep` (dependency list) to a non-existent reference -> expects BAD_REQUEST. |
    | 11 | `test_updateACTRdep` | CREATE a &lt;DEPR&gt; -> expects CREATED; UPDATE the &lt;ACTR&gt; setting `dep` to reference it -> expects UPDATED, checks `dep`; DELETE the &lt;DEPR&gt; -> expects DELETED. |
    | 12 | `test_ACTRDEPRsfcTrue` | Restart the &lt;ACTR&gt;'s periodic evaluation window; CREATE a &lt;DEPR&gt; (`sfc=True`, condition on `cbs`) and link it via the &lt;ACTR&gt;'s `dep` -> expects CREATED/UPDATED; CREATE a &lt;CIN&gt; (satisfying the dependency) -> expects CREATED; after a delay, RETRIEVE the &lt;AE&gt; -> expects OK, checks the action's label-setting effect (`lbl`) was applied; remove the label; DELETE the &lt;DEPR&gt; -> expects DELETED. |
    | 13 | `test_ACTRDEPRsfcFalse` | Same as `test_ACTRDEPRsfcTrue` but with `sfc=False` on the &lt;DEPR&gt; -> CREATE a &lt;CIN&gt;, checks the action still fires (label applied) since the single dependency is satisfied regardless of `sfc`; cleanup as before. |
    | 14 | `test_ACTRDEPRsfcTrueMissingConditionFail` | CREATE a &lt;DEPR&gt; (`sfc=True`) whose condition is on `lbl` (not satisfied by the upcoming &lt;CIN&gt; creation) and link it to the &lt;ACTR&gt; -> expects CREATED/UPDATED; CREATE a &lt;CIN&gt; -> expects CREATED; RETRIEVE the &lt;AE&gt; -> expects OK, checks NO label was applied (dependency condition not met); DELETE the &lt;DEPR&gt; -> expects DELETED. |
    | 15 | `test_ACTRDEPRsfcTrueAddedCondition` | Same setup as the missing-condition test (`sfc=True`, condition on `lbl`); after confirming no label is applied initially, UPDATE the &lt;CNT&gt; to add the matching `lbl` -> expects UPDATED; after a delay, RETRIEVE the &lt;AE&gt; -> expects OK, checks the label was now applied (condition newly satisfied); remove the label and the &lt;CNT&gt;'s label; DELETE the &lt;DEPR&gt; -> expects DELETED. |
    | 16 | `test_ACTRDEPRsfcFalseAddedCondition` | Same flow as the `sfc=True` added-condition test but with `sfc=False` on the &lt;DEPR&gt; -> checks the same entering-condition behavior (no label until the &lt;CNT&gt; label is added, then label applied); cleanup as before. |
    | 17 | `test_ACTRDEPRsfcFalseTwoDependenciesOneChangeFail` | CREATE 2 &lt;DEPR&gt;s with `sfc=False` (one condition on `lbl`, one on `cbs`) linked to the &lt;ACTR&gt;'s `dep` -> each expects CREATED/UPDATED; CREATE a &lt;CIN&gt; (satisfies only the `cbs` condition, not `lbl`) -> expects CREATED; RETRIEVE the &lt;AE&gt; -> expects OK, checks NO label applied (only one of two `sfc=False` dependencies met -- still need at least one transitions correctly, but in this scenario neither newly transitions to trigger); DELETE both &lt;DEPR&gt;s -> each expects DELETED. |
    | 18 | `test_ACTRDEPRsfcFalseTwoDependencies` | Same 2-dependency (`sfc=False`) setup; CREATE a &lt;CIN&gt; (satisfies the `cbs` condition) -> expects CREATED, checks NO label yet; UPDATE the &lt;CNT&gt; to add the matching `lbl` (now satisfies the 2nd condition too) -> expects UPDATED; after a delay, RETRIEVE the &lt;AE&gt; -> expects OK, checks the label was applied; cleanup label and DELETE both &lt;DEPR&gt;s -> each expects DELETED. |
