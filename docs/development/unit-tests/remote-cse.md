# Remote CSE / Inter-CSE

*4 test modules, 54 test cases. [&larr; Back to overview](index.md)*

??? note "`testRemote.py` — Remote CSE registration functionality (skipped if no remote CSE configured) (8 tests)"

    | # | Test Method | Requests Performed |
    |---|---|---|
    | 1 | `test_retrieveLocalCSR` | RETRIEVE the local &lt;CSR&gt; representing the registered remote CSE -> checks `ty`, `rn`, `ri`, `cb`, `csi` and that `poa` is a non-empty list. |
    | 2 | `test_retrieveRemoteCSR` | RETRIEVE (on the remote CSE) the &lt;CSR&gt; representing this local CSE's registration -> checks `ty`, `rn`, `ri`, `cb`, `csi` and that `poa` is a non-empty list. |
    | 3 | `test_createWithAEOriginatorFail` | CREATE a &lt;CSR&gt; under the local &lt;CSEBase&gt; using an AE-style originator (not an allowed CSR originator) -> expects OPERATION_NOT_ALLOWED. |
    | 4 | `test_createCSRmissingCSI` | CREATE a &lt;CSR&gt; with the `csi` attribute omitted -> expects BAD_REQUEST. |
    | 5 | `test_createCSRmissingCB` | CREATE a &lt;CSR&gt; with the `cb` attribute omitted -> expects BAD_REQUEST. |
    | 6 | `test_createCSRsameAsAE` | CREATE an &lt;AE&gt; with originator `Ctest` -> expects CREATED; CREATE a &lt;CSR&gt; with `cb` pointing to the same originator (`/Ctest`) -> expects CONFLICT (originator already used by an AE); DELETE the &lt;AE&gt; -> expects DELETED. |
    | 7 | `test_transferDiscoverOnRemoteCSE` | RETRIEVE a discovery query (`fu`/`drt` query params) on the remote CSE via the transfer (`~`) addressing scheme, using the remote CSE's own originator -> expects OK; checks the returned `uril` is a non-empty list whose entries are all prefixed with the remote CSE-ID. |
    | 8 | `test_transferDiscoverOnRemoteCSEWithLocalOriginatorFail` | RETRIEVE the same discovery query via the transfer addressing scheme but with the local originator -> expects OK, but checks the returned `uril` list is empty (no matching/authorized results). |

??? note "`testRemote_Annc.py` — Resource announcement to a remote CSE (43 tests)"

    | # | Test Method | Requests Performed |
    |---|---|---|
    | 1 | `test_createAnnounceAEwithATwithoutAA` | CREATE an &lt;AE&gt; with `at=[REMOTECSEID]` (announce target, no `aa`) -> expects CREATED; checks `at` has 1 entry pointing to the remote CSE and `aa` is absent. |
    | 2 | `test_retrieveAnnouncedAEwithATwithoutAA` | RETRIEVE the announced &lt;AEAnnc&gt; on the remote CSE -> expects OK; checks `ty=AEAnnc`, timestamps, `pi`, `lnk` points back to the original, and `srv` (mandatory-announced attribute) is present; records the remote `pi` as the CSEBaseAnnc reference. |
    | 3 | `test_retrieveCSEBaseAnnc` | RETRIEVE the &lt;CSEBaseAnnc&gt; on the remote CSE -> expects OK; checks `lnk`, `srv`, `ri`, `ty=CSEBaseAnnc`. |
    | 4 | `test_deleteAnnounceAE` | DELETE the original &lt;AE&gt; -> expects DELETED; RETRIEVE the announced copy on the remote CSE -> expects NOT_FOUND (unannounced on delete). |
    | 5 | `test_createAnnounceAEwithATwithWrongAA1Fail` | CREATE an &lt;AE&gt; with `aa` containing an attribute name with an invalid character (`+`) -> expects BAD_REQUEST. |
    | 6 | `test_createAnnounceAEwithATwithWrongAA2Fail` | CREATE an &lt;AE&gt; with `aa` containing an attribute name with a space -> expects BAD_REQUEST. |
    | 7 | `test_createAnnounceAEwithATwithWrongAA3Fail` | CREATE an &lt;AE&gt; with `aa` containing an attribute name starting with a digit -> expects BAD_REQUEST. |
    | 8 | `test_createAnnounceAEwithATwithAA` | CREATE an &lt;AE&gt; with `at` and `aa=['lbl']` -> expects CREATED; checks `lbl`, `at`, and `aa` all present and correctly populated. |
    | 9 | `test_retrieveAnnouncedAEwithATwithAA` | RETRIEVE the announced &lt;AEAnnc&gt; -> expects OK; checks `pi` matches the CSEBaseAnnc, `lnk`, `lbl` (the announced attribute) is present with the expected value, and `srv`. |
    | 10 | `test_addAAtoAnnounceAEwithoutAA` | UPDATE the &lt;AE&gt; adding `lbl` and `aa=['lbl']` -> expects UPDATED; checks `lbl`. |
    | 11 | `test_createAnnounceAEwithNAAttributes` | CREATE an &lt;AE&gt; with `aa` listing only non-announceable attributes (`ri`, `pi`, `ct`, `lt`, `acpi`) using `rcn=modifiedAttributes` -> expects CREATED; checks `at` has 1 entry and `aa` is present in the response body but is `None`/null (corrected to empty since none were actually announceable). |
    | 12 | `test_createAnnounceAEwithNonResourceAttributes` | CREATE an &lt;AE&gt; with `aa` including `rn` (a non-resource/structural attribute not eligible for announcement) -> expects BAD_REQUEST. |
    | 13 | `test_addLBLtoAnnouncedAE` | UPDATE the &lt;AE&gt; setting `lbl` (a conditionally-announced attribute, no explicit `aa` needed) -> expects UPDATED; checks `lbl`; RETRIEVE the announced copy -> expects OK, checks `lbl` propagated. |
    | 14 | `test_removeLBLfromAnnouncedAE` | UPDATE the &lt;AE&gt; removing `lbl` (`None`) -> expects UPDATED; checks `lbl` absent; RETRIEVE the announced copy -> expects OK, checks `lbl` is also absent there. |
    | 15 | `test_createAnnounceNode` | CREATE a &lt;NOD&gt; with `at` -> expects CREATED; checks `at` has 1 entry and `aa` absent. |
    | 16 | `test_retrieveAnnouncedNode` | RETRIEVE the announced &lt;NODAnnc&gt; -> expects OK; checks `ty=NODAnnc`, timestamps, `pi` matches CSEBaseAnnc, `lnk`. |
    | 17 | `test_announceMgmtobj` | CREATE a &lt;BAT&gt; (mgmtObj) under the &lt;NOD&gt; with `at` and `aa=['btl']` -> expects CREATED; checks `at`/`btl`/`bts`/`aa`. |
    | 18 | `test_retrieveAnnouncedMgmtobj` | RETRIEVE the announced &lt;BATAnnc&gt; -> expects OK; checks `ty=MGMTOBJAnnc`, `mgd=BAT`, timestamps, `pi`, `lnk`, `btl` (announced) present, `bts` (not announced) absent. |
    | 19 | `test_retrieveRCNOriginalResource` | RETRIEVE the announced &lt;BAT&gt; with `rcn=originalResource` using the remote CSE-ID as originator -> expects OK; checks the response is the actual original `m2m:bat` resource (not the announced shadow), with `ty=MGMTOBJ`, `mgd=BAT`, `aa`, `at`. |
    | 20 | `test_retrieveRCNOriginalResourceFail` | RETRIEVE the remote CSEBase itself with `rcn=originalResource` -> expects BAD_REQUEST (not applicable to CSEBase). |
    | 21 | `test_deleteRCNOriginalResourceFail` | DELETE the announced &lt;BAT&gt; with `rcn=originalResource` -> expects BAD_REQUEST. |
    | 22 | `test_updateMgmtObjAttribute` | UPDATE the original &lt;BAT&gt; setting `btl=42`, `bts=2` -> expects UPDATED; RETRIEVE the announced copy -> expects OK, checks `btl` propagated but `bts` (not in `aa`) absent. |
    | 23 | `test_addMgmtObjAttribute` | UPDATE the &lt;BAT&gt; adding `bts` to `aa` -> expects UPDATED, checks `aa` now has 2 entries; RETRIEVE the announced copy -> expects OK, checks both `btl` and `bts` now propagated. |
    | 24 | `test_removeMgmtObjAttribute` | UPDATE the &lt;BAT&gt; setting `aa=['bts']` (removing `btl` from the announced set) -> expects UPDATED, checks `aa` now has 1 entry; RETRIEVE the announced copy -> expects OK, checks `btl` removed but `bts` remains. |
    | 25 | `test_removeMgmtObjAA` | UPDATE the &lt;BAT&gt; setting `aa=None` (unannounce all attributes) -> expects UPDATED, checks `aa` absent; RETRIEVE the announced copy -> expects OK, checks both `btl`/`bts` are now absent there. |
    | 26 | `test_removeMgmtObjCSIfromAT` | UPDATE the &lt;BAT&gt; setting `at=None` (full unannounce) -> expects UPDATED, checks `at` absent; RETRIEVE the announced copy -> expects NOT_FOUND. |
    | 27 | `test_addMgmtObjCSItoAT` | UPDATE the &lt;BAT&gt; re-adding `at=[REMOTECSEID]` (re-announce) -> expects UPDATED, checks `at` has 1 entry; RETRIEVE the new announced copy -> expects OK. |
    | 28 | `test_removeMgmtObjAT` | UPDATE the &lt;BAT&gt; setting `at=None` again -> expects UPDATED, checks `at` absent; RETRIEVE the announced copy -> expects NOT_FOUND. |
    | 29 | `test_deleteAnnounceNode` | DELETE the &lt;NOD&gt; -> expects DELETED; RETRIEVE the announced &lt;NODAnnc&gt; -> expects NOT_FOUND; RETRIEVE the announced &lt;BATAnnc&gt; (already cascaded with its parent) -> expects NOT_FOUND. |
    | 30 | `test_createAnnouncedACP` | CREATE an &lt;ACP&gt; with `at` -> expects CREATED; checks `at` has 1 entry and `aa` absent. |
    | 31 | `test_retrieveAnnouncedACPwithCSERelativeOriginatorFail` | RETRIEVE the announced &lt;ACPAnnc&gt; using a bare CSE-relative (non-prefixed) originator -> expects ORIGINATOR_HAS_NO_PRIVILEGE. |
    | 32 | `test_retrieveAnnouncedACP` | RETRIEVE the announced &lt;ACPAnnc&gt; with an unrelated originator -> expects ORIGINATOR_HAS_NO_PRIVILEGE; RETRIEVE again using a CSE-relative authorized originator -> expects OK, checks `ty=ACPAnnc`, timestamps, `pi`, `lnk`, `pv`/`pvs` (mandatory-announced attributes). |
    | 33 | `test_retrieveAnnouncedACPwithWrongOriginator` | RETRIEVE the announced &lt;ACPAnnc&gt; with an unauthorized originator -> expects ORIGINATOR_HAS_NO_PRIVILEGE. |
    | 34 | `test_retrieveAnnouncedACPwithCSI` | RETRIEVE the announced &lt;ACPAnnc&gt; using the remote CSE's own CSE-ID as originator -> expects OK. |
    | 35 | `test_deleteAnnounceACP` | DELETE the original &lt;ACP&gt; -> expects DELETED; RETRIEVE the announced copy -> expects NOT_FOUND. |
    | 36 | `test_createAnnouncedCNTSynced` | CREATE a &lt;CNT&gt; with `at`, `aa=['mni']`, and `ast=BI_DIRECTIONAL` (bidirectional sync) -> expects CREATED; checks `at` has 1 entry. |
    | 37 | `test_updateRemoteCNT` | UPDATE the local &lt;CNT&gt; setting `lbl` and `mni=20` -> expects UPDATED; checks `lbl` and `mni` reflect the change. |
    | 38 | `test_deleteAnnouncedCNTSynced` | DELETE the &lt;CNT&gt; -> expects DELETED; RETRIEVE the announced copy -> expects NOT_FOUND. |
    | 39 | `test_announceToHostingCSE` | CREATE a &lt;CNT&gt; announcing to its own hosting CSE (`at=[CSEID]`, self-announcement) -> expects CREATED; checks `at`; RETRIEVE the locally-announced shadow resource -> expects OK; UPDATE the original &lt;CNT&gt;'s `lbl` -> expects UPDATED, checks 2 labels now present; RETRIEVE the locally announced copy -> expects OK, checks `lbl` has 2 entries too. |
    | 40 | `test_unannounceFromHostingCSE` | DELETE the self-announced &lt;CNT&gt; -> expects DELETED. |
    | 41 | `test_announceAEwithIdentifierAttributes` | CREATE an &lt;AE&gt; with `aa=['aei']` (announcing an identifier attribute) -> expects CREATED; DELETE it -> expects DELETED. |
    | 42 | `test_announceFCNT` | CREATE an &lt;AE&gt; -> expects CREATED; CREATE a &lt;FCNT&gt; (`cod:tempe`) with `aa=['curT0']` and `at` -> expects CREATED, checks `at`; RETRIEVE the announced &lt;FCNTAnnc&gt; -> expects OK, checks `curT0` propagated; DELETE the &lt;FCNT&gt; and the &lt;AE&gt; -> each expects DELETED. |
    | 43 | `test_announceFCNTUpdate` | CREATE an &lt;AE&gt; -> expects CREATED; CREATE a &lt;FCNT&gt; with `aa=['curT0']`, `at`, and both `curT0`/`tarTe` -> expects CREATED; RETRIEVE the announced copy -> expects OK, checks `curT0` present but `tarTe` absent (not yet announced); UPDATE the &lt;FCNT&gt; to add `tarTe` to `aa` -> expects UPDATED; RETRIEVE the announced copy again -> expects OK, checks both `curT0` and `tarTe` now present; DELETE the &lt;FCNT&gt; and &lt;AE&gt; -> each expects DELETED. |

??? note "`testRemote_GRP.py` — Group announcement/fanOut to a remote CSE (2 tests)"

    | # | Test Method | Requests Performed |
    |---|---|---|
    | 1 | `test_createGrp` | CREATE a &lt;GRP&gt; under the local &lt;AE&gt;, originator set to the local CSE-ID, with `mid` containing both the local CSEBase and the remote CSR -> expects CREATED; checks the returned `mid` list contains exactly those two members. |
    | 2 | `test_retrieveFOPT` | RETRIEVE the group's fanOutPoint (`&lt;GRP&gt;/fopt`) with the CSE-ID as originator -> expects OK; verifies the aggregated response contains exactly 2 results (one from the local CSEBase, one from the remote CSR), each with the matching CSE-ID. |

??? note "`testRemote_Requests.py` — Various requests routed to/through a remote CSE (1 tests)"

    | # | Test Method | Requests Performed |
    |---|---|---|
    | 1 | `test_forwardCreateCNT` | CREATE an &lt;AE&gt; directly on the remote CSE (targeting the remote CSE-relative root `-` with a CSE-relative originator) -> expects CREATED; CREATE a &lt;CNT&gt; under that remote &lt;AE&gt; via a request forwarded through the local CSE -> expects CREATED; DELETE the remote &lt;AE&gt; (and its &lt;CNT&gt;) via the same forwarding path -> expects DELETED. |
