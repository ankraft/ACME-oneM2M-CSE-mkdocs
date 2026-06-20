# Access Control & Security

*2 test modules, 72 test cases. [&larr; Back to overview](index.md)*

??? note "`testACP.py` — Access Control Policy (&lt;ACP&gt;) resource lifecycle and permission evaluation (61 tests)"

    | # | Test Method | Requests Performed |
    |---|---|---|
    | 1 | `test_createACP` | CREATE an &lt;ACP&gt; under &lt;CSEBase&gt; with `pv` listing 5 originators (including 2 wildcard patterns) and `pvs` listing 2 -> expects CREATED. |
    | 2 | `test_retrieveACP` | RETRIEVE the &lt;ACP&gt; -> expects OK. |
    | 3 | `test_retrieveACPwrongOriginator` | RETRIEVE the &lt;ACP&gt; with an unauthorized originator -> expects ORIGINATOR_HAS_NO_PRIVILEGE. |
    | 4 | `test_attributesACP` | RETRIEVE the &lt;ACP&gt; -> expects OK; checks `ty`, timestamps ordering, `pv.acr` (originators/`acop=ALL`), and `pvs.acr` (originators/`acop=ALL`) all match what was created. |
    | 5 | `test_updateACP` | UPDATE the &lt;ACP&gt; setting `lbl` -> expects UPDATED; checks `lbl`. |
    | 6 | `test_updateACPwrongOriginator` | UPDATE the &lt;ACP&gt; with an unauthorized originator -> expects ORIGINATOR_HAS_NO_PRIVILEGE. |
    | 7 | `test_addACPtoAE` | CREATE an &lt;AE&gt; referencing the &lt;ACP&gt; via `acpi` -> expects CREATED; checks `acpi` contains the ACP's `ri`. |
    | 8 | `test_updateAEACPIWrong` | UPDATE the &lt;AE&gt; setting both `lbl` and `acpi` together (not allowed in combination) -> expects something other than UPDATED. |
    | 9 | `test_updateAEACPIWrong2` | UPDATE the &lt;AE&gt; setting `acpi` to a non-existent ACP reference -> expects something other than UPDATED. |
    | 10 | `test_updateAEACPIWrongOriginator` | UPDATE the &lt;AE&gt;'s `acpi` using an originator not authorized on the referenced ACP -> expects something other than UPDATED. |
    | 11 | `test_updateAEACPIOtherOriginator` | UPDATE the &lt;AE&gt;'s `acpi` using a second authorized originator -> expects UPDATED. |
    | 12 | `test_updateAElblWithWildCardOriginator` | UPDATE the &lt;AE&gt;'s `lbl` using an originator matching a wildcard ACP pattern (`Canother*`) -> expects UPDATED; checks `lbl`. |
    | 13 | `test_updateAElblWithWildCardOriginator2` | Same but using an originator matching a different wildcard pattern (`Cyet*Originator`) -> expects UPDATED; checks `lbl`. |
    | 14 | `test_updateAElblWithWildCardOriginator3WrongFail` | Same but with an originator that does NOT match either wildcard pattern -> expects ORIGINATOR_HAS_NO_PRIVILEGE. |
    | 15 | `test_updateACPEmptyPVSFail` | UPDATE the &lt;ACP&gt; setting `pvs={}` (empty) -> expects BAD_REQUEST. |
    | 16 | `test_updateACPNoPVSFail` | UPDATE the &lt;ACP&gt; setting `pvs=None` (removing self-privileges) -> expects BAD_REQUEST. |
    | 17 | `test_createACPNoPVSFail` | CREATE an &lt;ACP&gt; with no `pvs` at all -> expects BAD_REQUEST. |
    | 18 | `test_createACPEmptyPVSFail` | CREATE an &lt;ACP&gt; with `pvs={}` -> expects BAD_REQUEST. |
    | 19 | `test_createCNTwithNoACPI` | CREATE a &lt;CNT&gt; with no `acpi` -> expects CREATED; checks `acpi` absent. |
    | 20 | `test_retrieveCNTwithNoACPI` | RETRIEVE the &lt;CNT&gt; (no ACP, falls back to parent's permissions) with the admin originator -> expects OK. |
    | 21 | `test_retrieveCNTwithNoACPIWrongOriginator` | RETRIEVE the same &lt;CNT&gt; with an unauthorized originator -> expects ORIGINATOR_HAS_NO_PRIVILEGE. |
    | 22 | `test_deleteCNTwithNoACPI` | DELETE the &lt;CNT&gt; -> expects DELETED. |
    | 23 | `test_createCNTwithNoACPIAndCustodian` | CREATE a &lt;CNT&gt; with no `acpi` but a `cstn` (custodian) set -> expects CREATED. |
    | 24 | `test_retrieveCNTwithNoACPIAndCustodian` | RETRIEVE the &lt;CNT&gt; using the custodian's originator -> expects OK. |
    | 25 | `test_retrieveCNTwithNoACPIAndCustodianAEOriginator` | RETRIEVE the same &lt;CNT&gt; using the parent &lt;AE&gt;'s originator (not the custodian) -> expects ORIGINATOR_HAS_NO_PRIVILEGE. |
    | 26 | `test_retrieveCNTwithNoACPIAndCustodianWrongOriginator` | RETRIEVE the &lt;CNT&gt; with a wholly unrelated originator -> expects ORIGINATOR_HAS_NO_PRIVILEGE. |
    | 27 | `test_deleteCNTwithNoACPIAndCustodian` | DELETE the &lt;CNT&gt; using the custodian's originator -> expects DELETED. |
    | 28 | `test_removeACPfromAEWrong` | UPDATE the &lt;AE&gt; removing the ACP reference from `acpi` while also keeping other entries but as the only attribute besides `pvs` issue (effectively malformed) -> expects BAD_REQUEST. |
    | 29 | `test_removeACPfromAEWrong2` | UPDATE the &lt;AE&gt; setting `acpi=None` using the AE's own originator (which would remove its own self-privileges) -> expects ORIGINATOR_HAS_NO_PRIVILEGE. |
    | 30 | `test_removeACPfromAE` | UPDATE the &lt;AE&gt; setting `acpi=None` using an authorized ACP originator -> expects UPDATED. |
    | 31 | `test_deleteACPwrongOriginator` | DELETE the &lt;ACP&gt; with an unauthorized originator -> expects ORIGINATOR_HAS_NO_PRIVILEGE. |
    | 32 | `test_deleteACP` | DELETE the &lt;ACP&gt; with the correct originator -> expects DELETED. |
    | 33 | `test_createACPUnderCSEBaseWithOriginator` | CREATE an &lt;ACP&gt; directly under &lt;CSEBase&gt; using the &lt;AE&gt;'s own originator, with an empty `pv.acr` -> expects CREATED. |
    | 34 | `test_deleteACPUnderCSEBaseWithOriginator` | DELETE that &lt;ACP&gt; using the same originator -> expects DELETED. |
    | 35 | `test_createACPUnderAEWithChty` | CREATE an &lt;ACP&gt; under the &lt;AE&gt; with `acod.chty=[CNT]` (restrict child-create to CNT type only) -> expects CREATED; checks `acod.chty`. |
    | 36 | `test_updateAEACPIForChty` | UPDATE the &lt;AE&gt; to reference that ACP via `acpi` -> expects UPDATED; checks `acpi`. |
    | 37 | `test_testACPChty` | CREATE a &lt;CNT&gt; under the &lt;AE&gt; (allowed by `chty`) -> expects CREATED; CREATE a &lt;FCNT&gt; (not allowed by `chty`) -> expects ORIGINATOR_HAS_NO_PRIVILEGE. |
    | 38 | `test_deleteACPUnderAEWithChty` | DELETE the chty-restricted &lt;ACP&gt; -> expects DELETED. |
    | 39 | `test_accessCINwithDifferentAENoAcpi` | Set up 2 &lt;AE&gt;s, an &lt;ACP&gt; and a &lt;CNT&gt;/&lt;CIN&gt; under the 2nd &lt;AE&gt; (no `acpi` on the CNT) -> all CREATEs expect CREATED; RETRIEVE the &lt;CIN&gt;'s `la` using the 1st &lt;AE&gt;'s originator -> expects ORIGINATOR_HAS_NO_PRIVILEGE. |
    | 40 | `test_accessCINwithDifferentAEWithAcpi` | UPDATE the &lt;CNT&gt; to add the &lt;ACP&gt; (which grants the 1st AE access) via `acpi` -> expects UPDATED; RETRIEVE the &lt;CIN&gt;'s `la` with the 1st AE's originator -> expects OK. |
    | 41 | `test_discoverCINwithDifferentAEWithAcpi` | Discover &lt;CIN&gt;s (`fu=1&ty=4`) under the &lt;CNT&gt; using the 1st AE's originator -> expects OK; checks exactly 1 matching result. |
    | 42 | `test_retrieveACPwithoutRETRIEVEAccessFail` | CREATE an &lt;ACP&gt; whose `pvs` grants all permissions except RETRIEVE -> expects CREATED; RETRIEVE the &lt;ACP&gt; itself using that originator -> expects ORIGINATOR_HAS_NO_PRIVILEGE. |
    | 43 | `test_createACPWithWrongTyFail` | CREATE an &lt;ACP&gt; with `acod.ty` given as a single value instead of a list -> expects BAD_REQUEST. |
    | 44 | `test_createACPWithTy` | CREATE an &lt;ACP&gt; with `acod.ty=CNT` (correct list-free form accepted) and `acod.chty=[CNT]` -> expects CREATED; checks `acod.chty`; CREATE a &lt;CNT&gt; referencing it -> expects CREATED; attempt to CREATE a &lt;CIN&gt; under it (RETRIEVE+CREATE granted, but CIN not in `chty`) -> expects ORIGINATOR_HAS_NO_PRIVILEGE; RETRIEVE the &lt;CNT&gt; -> expects OK; CREATE a nested &lt;CNT&gt; (matches `acod.ty`/`chty`) -> expects CREATED. |
    | 45 | `test_testACPacorGRP` | Set up an &lt;AE&gt;, a &lt;GRP&gt; containing the AE as a member, an &lt;ACP&gt; whose `pv.acor` references the &lt;GRP&gt; itself, and a &lt;CNT&gt; using that ACP -> all CREATEs expect CREATED; CREATE a &lt;CIN&gt; using the AE's originator (group member) -> expects CREATED; CREATE a &lt;CIN&gt; with an unrelated originator -> expects ORIGINATOR_HAS_NO_PRIVILEGE; cleanup DELETEs (not checked). |
    | 46 | `test_createACPwithACA` | CREATE an &lt;AE&gt;; CREATE an &lt;ACP&gt; with `pv.acr.aca=['lbl']` (attribute-restricted access) -> expects CREATED; checks `aca`; DELETE the &lt;AE&gt; (cascading the ACP) -> expects DELETED. |
    | 47 | `test_updateACPwithACA` | Same setup, re-verifying `aca` creation behavior -> expects CREATED, checks `aca`; cleanup DELETE -> expects DELETED. |
    | 48 | `test_createACPwithACARetrieveCntFail` | CREATE an &lt;AE&gt;, an &lt;ACP&gt; with `aca=['lbl']` for a different originator, and a &lt;CNT&gt; using it -> all expect CREATED; RETRIEVE the full &lt;CNT&gt; with that originator (full retrieve not allowed when `aca` restricts to non-RETRIEVE-relevant attrs) -> expects ORIGINATOR_HAS_NO_PRIVILEGE; cleanup DELETE -> expects DELETED. |
    | 49 | `test_createACPwithACARetrieveCnt` | Same setup but `aca` lists nearly all attributes -> CREATE all expect CREATED; RETRIEVE the full &lt;CNT&gt; -> expects OK; cleanup DELETE -> expects DELETED. |
    | 50 | `test_createACPwithACARetrieveCntPartial` | Same setup with `aca=['lbl']` only -> CREATE all expect CREATED; partial RETRIEVE (`atrl=lbl`) -> expects OK; cleanup DELETE -> expects DELETED. |
    | 51 | `test_createACPwithACARetrieveCntPartialFail` | Same `aca=['lbl']` setup -> partial RETRIEVE of a different attribute (`atrl=rn`, not in `aca`) -> expects ORIGINATOR_HAS_NO_PRIVILEGE; cleanup DELETE -> expects DELETED. |
    | 52 | `test_createACPwithACAAndSimpleACPRetrieveCntPartial` | CREATE 2 ACPs (one with `aca=['lbl']`, one plain full-RETRIEVE) both referenced on the same &lt;CNT&gt; -> all expect CREATED; partial RETRIEVE of `rn` -> expects OK (the simple ACP grants it); cleanup DELETE -> expects DELETED. |
    | 53 | `test_createACPwithACADeleteCntFail` | Same `aca=['lbl']` pattern but with `acop=DELETE` -> DELETE the &lt;CNT&gt; with that originator -> expects ORIGINATOR_HAS_NO_PRIVILEGE; cleanup DELETE -> expects DELETED. |
    | 54 | `test_createACPwithACADeleteCnt` | Same but `aca` lists all attributes -> DELETE the &lt;CNT&gt; -> expects DELETED; cleanup DELETE of the AE -> expects DELETED. |
    | 55 | `test_createACPwithACACREATECntFail` | ACP with `acop=CREATE`, `aca=['lbl','rn']` -> CREATE a nested &lt;CNT&gt; with an attribute (`mni`) not in `aca` -> expects ORIGINATOR_HAS_NO_PRIVILEGE; cleanup DELETE -> expects DELETED. |
    | 56 | `test_createACPwithACACREATECnt` | Same but `aca` also includes `mni` -> CREATE the nested &lt;CNT&gt; with `mni` -> expects CREATED, checks only `rn`/`lbl`/`mni` are present in the response (CSE-internal attributes suppressed); cleanup DELETE -> expects DELETED. |
    | 57 | `test_createACPwithACAUPDATECntFail` | ACP with `acop=UPDATE`, `aca=['lbl']` -> UPDATE the &lt;CNT&gt;'s `mni` (not in `aca`) -> expects ORIGINATOR_HAS_NO_PRIVILEGE; cleanup DELETE -> expects DELETED. |
    | 58 | `test_createACPwithACAUPDATECnt` | Same but UPDATE `lbl` (in `aca`) -> expects UPDATED; cleanup DELETE -> expects DELETED. |
    | 59 | `test_createACPwithACAFRetrieveCnt` | (HTTP Basic Auth only) CREATE an &lt;ACP&gt; with `acaf` (authenticated-only access flag) for a different originator, and a &lt;CNT&gt; using it -> expect CREATED; RETRIEVE the &lt;CNT&gt; -> expects OK if HTTP/token/OAuth authentication is configured, otherwise ORIGINATOR_HAS_NO_PRIVILEGE; cleanup DELETE -> expects DELETED. |
    | 60 | `test_createACPwithACTWRetrieveCnt` | CREATE an &lt;AE&gt;, an &lt;ACP&gt; with `acco.actw` (access control time window) set to an always-matching cron expression, and a &lt;CNT&gt; using it -> all expect CREATED; RETRIEVE the &lt;CNT&gt; -> expects OK (inside the window); cleanup DELETE -> expects DELETED. |
    | 61 | `test_createACPwithACTWRetrieveCntFail` | Same but `actw` set to a cron expression that never matches (year 1984) -> RETRIEVE the &lt;CNT&gt; -> expects ORIGINATOR_HAS_NO_PRIVILEGE (outside the window); cleanup DELETE -> expects DELETED. |

??? note "`testDAC.py` — Dynamic Authorization Consultation (DAC) functionality (11 tests)"

    | # | Test Method | Requests Performed |
    |---|---|---|
    | 1 | `test_createDACunderCSEFail` | CREATE a &lt;DAC&gt; under the &lt;CSEBase&gt; missing the mandatory `dap` attribute -> expects BAD_REQUEST. |
    | 2 | `test_createDACunderCSE` | CREATE a &lt;DAC&gt; under the &lt;CSEBase&gt; with `dae=False` and `dap=['aURL']` -> expects CREATED. |
    | 3 | `test_retrieveDACunderCSE` | RETRIEVE the &lt;DAC&gt; under the &lt;CSEBase&gt; -> expects OK; checks `rn`, `dae`, `dap` match what was created. |
    | 4 | `test_updateDACunderCSEFail` | UPDATE the &lt;DAC&gt; setting `dap` to `None` (removing the mandatory attribute) -> expects BAD_REQUEST. |
    | 5 | `test_updateDACunderCSE2Fail` | UPDATE the &lt;DAC&gt; setting `dap` to an empty list -> expects BAD_REQUEST. |
    | 6 | `test_updateDACunderCSE` | UPDATE the &lt;DAC&gt; setting `dae=True`, a new `dap` list, and `dal` (a future timestamp) -> expects UPDATED; RETRIEVE it -> expects OK, checks all updated attributes. |
    | 7 | `test_deleteDACunderCSE` | DELETE the &lt;DAC&gt; under the &lt;CSEBase&gt; -> expects DELETED. |
    | 8 | `test_createDACunderAE` | CREATE a &lt;DAC&gt; under the &lt;AE&gt; with `dae=False`, `dap=['aURL']` -> expects CREATED. |
    | 9 | `test_retrieveDACunderAE` | RETRIEVE the &lt;DAC&gt; under the &lt;AE&gt; -> expects OK; checks `rn`, `dae`, `dap`. |
    | 10 | `test_updateDACunderAE` | UPDATE the &lt;DAC&gt; under the &lt;AE&gt; setting `dae=True`, new `dap`, and `dal` -> expects UPDATED; RETRIEVE it -> expects OK, checks updated attributes. |
    | 11 | `test_deleteDACunderAE` | DELETE the &lt;DAC&gt; under the &lt;AE&gt; -> expects DELETED. |
