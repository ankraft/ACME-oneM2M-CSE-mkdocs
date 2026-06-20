# Management, Location, Scheduling & Policies

*6 test modules, 221 test cases. [&larr; Back to overview](index.md)*

??? note "`testMgmtObj.py` — All &lt;mgmtObj&gt; specializations (FWR, SWR, MEM, ANI, ANDI, BAT, DVI, DVC, RBO, EVL, NYCFC, etc.) (91 tests)"

    | # | Test Method | Requests Performed |
    |---|---|---|
    | 1 | `test_createFWR` | CREATE a &lt;FWR&gt; (Firmware mgmtObj) under the &lt;NOD&gt; with `dc`, `vr`, `fwn`, `url`, `ud` -> expects CREATED; checks `ri`. |
    | 2 | `test_retrieveFWR` | RETRIEVE the &lt;FWR&gt; -> expects OK; checks `mgd=FWR`. |
    | 3 | `test_attributesFWR` | RETRIEVE the &lt;FWR&gt; -> expects OK; checks `ty`, `pi`, `rn`, timestamps, `dc`, `vr`, `fwn`, `url`, `ud`, and `uds` (update status, a dict). |
    | 4 | `test_deleteFWR` | DELETE the &lt;FWR&gt; -> expects DELETED. |
    | 5 | `test_createSWR` | CREATE a &lt;SWR&gt; (Software) with `dc`, `vr`, `swn`, `url` -> expects CREATED; checks `ri`. |
    | 6 | `test_retrieveSWR` | RETRIEVE the &lt;SWR&gt; -> expects OK; checks `mgd=SWR`. |
    | 7 | `test_attributesSWR` | RETRIEVE the &lt;SWR&gt; -> expects OK; checks `ty`, `pi`, `rn`, timestamps, `dc`, `vr`, `swn`, `url`, and presence of `in`/`un`/`ins` (install/uninstall status fields). |
    | 8 | `test_deleteSWR` | DELETE the &lt;SWR&gt; -> expects DELETED. |
    | 9 | `test_createMEM` | CREATE a &lt;MEM&gt; (Memory) with `dc`, `mma`, `mmt` -> expects CREATED; checks `ri`. |
    | 10 | `test_retrieveMEM` | RETRIEVE the &lt;MEM&gt; -> expects OK; checks `mgd=MEM`. |
    | 11 | `test_attributesMEM` | RETRIEVE the &lt;MEM&gt; -> expects OK; checks `ty`, `pi`, `rn`, timestamps, `dc`, `mma`, `mmt`. |
    | 12 | `test_deleteMEM` | DELETE the &lt;MEM&gt; -> expects DELETED. |
    | 13 | `test_createANI` | CREATE a &lt;ANI&gt; (areaNwkInfo) with `dc`, `ant`, `ldv` (list) -> expects CREATED; checks `ri`. |
    | 14 | `test_retrieveANI` | RETRIEVE the &lt;ANI&gt; -> expects OK; checks `mgd=ANI`. |
    | 15 | `test_attributesANI` | RETRIEVE the &lt;ANI&gt; -> expects OK; checks `ty`, `pi`, `rn`, timestamps, `dc`, `ant`, and `ldv` (2-item list). |
    | 16 | `test_deleteANI` | DELETE the &lt;ANI&gt; -> expects DELETED. |
    | 17 | `test_createANDI` | CREATE a &lt;ANDI&gt; (areaNwkDeviceInfo) with `dc`, `dvd`, `dvt`, `awi`, `sli`, `sld`, `lnh` (list) -> expects CREATED; checks `ri`. |
    | 18 | `test_retrieveANDI` | RETRIEVE the &lt;ANDI&gt; -> expects OK; checks `mgd=ANDI`. |
    | 19 | `test_attributesANDI` | RETRIEVE the &lt;ANDI&gt; -> expects OK; checks `ty`, `pi`, `rn`, timestamps, `dc`, `dvd`, `dvt`, `awi`, `sli`, `sld`, `lnh` (2-item list). |
    | 20 | `test_deleteANDI` | DELETE the &lt;ANDI&gt; -> expects DELETED. |
    | 21 | `test_createBATWrong` | CREATE a &lt;BAT&gt; (battery) with an out-of-range `bts` (battery status, `99`) -> expects BAD_REQUEST. |
    | 22 | `test_createBAT` | CREATE a &lt;BAT&gt; with valid `dc`, `btl`, `bts` -> expects CREATED; checks `ri`/`ty=MGMTOBJ`. |
    | 23 | `test_retrieveBAT` | RETRIEVE the &lt;BAT&gt; -> expects OK; checks `mgd=BAT`. |
    | 24 | `test_attributesBAT` | RETRIEVE the &lt;BAT&gt; -> expects OK; checks `ty`, `pi`, `rn`, timestamps, `dc`, `btl`, `bts`. |
    | 25 | `test_deleteBAT` | DELETE the &lt;BAT&gt; -> expects DELETED. |
    | 26 | `test_createDVI` | CREATE a &lt;DVI&gt; (deviceInfo) with a full set of device-description attributes (`dlb`, `man`, `mfdl`, `mfd`, `mod`, `smod`, `dty`, `dvnm`, `fwv`, `swv`, `hwv`, `osv`, `cnty`, `loc`, `syst`, `spur`, `purl`, `ptl`) -> expects CREATED; checks `ri`. |
    | 27 | `test_retrieveDVI` | RETRIEVE the &lt;DVI&gt; -> expects OK; checks `mgd=DVI`. |
    | 28 | `test_attributesDVI` | RETRIEVE the &lt;DVI&gt; -> expects OK; checks `ty`, `pi`, `rn`, timestamps, and all the device-description attributes match what was created. |
    | 29 | `test_deleteDVI` | DELETE the &lt;DVI&gt; -> expects DELETED. |
    | 30 | `test_createDVC` | CREATE a &lt;DVC&gt; (deviceCapability) with `can`, `att`, `cas` (action/status dict), `cus` -> expects CREATED; checks `ri`. |
    | 31 | `test_retrieveDVC` | RETRIEVE the &lt;DVC&gt; -> expects OK; checks `mgd=DVC`. |
    | 32 | `test_attributesDVC` | RETRIEVE the &lt;DVC&gt; -> expects OK; checks `ty`, `pi`, `rn`, timestamps, `dc`, `can`, `att`, `cas.acn`/`cas.sus`, `cus`, and that `ena`/`dis` are both true. |
    | 33 | `test_updateDVCEnaTrue` | UPDATE the &lt;DVC&gt; setting `ena=True` -> expects UPDATED; checks `ena`/`dis` both still true. |
    | 34 | `test_updateDVCEnaFalse` | UPDATE the &lt;DVC&gt; setting `ena=False` -> expects UPDATED; checks `ena`/`dis` both true (CSE resets them). |
    | 35 | `test_updateDVCDisTrue` | UPDATE the &lt;DVC&gt; setting `dis=True` -> expects UPDATED; checks `ena`/`dis` both true. |
    | 36 | `test_updateDVCDisFalse` | UPDATE the &lt;DVC&gt; setting `dis=False` -> expects UPDATED; checks `ena`/`dis` both true. |
    | 37 | `test_updateDVCEnaDisTrue` | UPDATE the &lt;DVC&gt; setting both `ena=True` and `dis=True` simultaneously -> expects BAD_REQUEST (mutually exclusive). |
    | 38 | `test_updateDVCEnaDisFalse` | UPDATE the &lt;DVC&gt; setting both `ena=False` and `dis=False` -> expects UPDATED; checks `ena`/`dis` both reset to true. |
    | 39 | `test_deleteDVC` | DELETE the &lt;DVC&gt; -> expects DELETED. |
    | 40 | `test_createRBO` | CREATE a &lt;RBO&gt; (reboot) with `rbo=False`, `far=False` -> expects CREATED; checks `ri`. |
    | 41 | `test_retrieveRBO` | RETRIEVE the &lt;RBO&gt; -> expects OK; checks `mgd=RBO`. |
    | 42 | `test_attributesRBO` | RETRIEVE the &lt;RBO&gt; -> expects OK; checks `ty`, `pi`, `rn`, timestamps, `dc`, `rbo=False`, `far=False`. |
    | 43 | `test_updateRBORboTrue` | UPDATE the &lt;RBO&gt; setting `rbo=True` (trigger reboot) -> expects UPDATED; checks `rbo`/`far` both reset to false (action consumed). |
    | 44 | `test_updateRBORboFalse` | UPDATE the &lt;RBO&gt; setting `rbo=False` -> expects UPDATED; checks `rbo`/`far` both false. |
    | 45 | `test_updateRBOFarTrue` | UPDATE the &lt;RBO&gt; setting `far=True` (factory reset) -> expects UPDATED; checks `rbo`/`far` both reset to false. |
    | 46 | `test_updateRBOFarFalse` | UPDATE the &lt;RBO&gt; setting `far=False` -> expects UPDATED; checks `rbo`/`far` both false. |
    | 47 | `test_updateRBORboFarTrue` | UPDATE the &lt;RBO&gt; setting both `rbo=True` and `far=True` simultaneously -> expects BAD_REQUEST. |
    | 48 | `test_updateRBORboFarFalse` | UPDATE the &lt;RBO&gt; setting both `rbo=False` and `far=False` -> expects UPDATED; checks both false. |
    | 49 | `test_deleteRBO` | DELETE the &lt;RBO&gt; -> expects DELETED. |
    | 50 | `test_createNYCFCwrongSUID` | CREATE a &lt;NYCFC&gt; (myCertFileCred) with an invalid `suids` entry (`99`) -> expects BAD_REQUEST. |
    | 51 | `test_createNYCFC` | CREATE a &lt;NYCFC&gt; with valid `suids`, `mcff`, `mcfc` -> expects CREATED; checks `ri`. |
    | 52 | `test_retrieveNYCFC` | RETRIEVE the &lt;NYCFC&gt; -> expects OK; checks `mgd=NYCFC`. |
    | 53 | `test_attributesNYCFC` | RETRIEVE the &lt;NYCFC&gt; -> expects OK; checks `ty`, `pi`, `rn`, timestamps, `dc`, `suids`, `mcff`, `mcfc`. |
    | 54 | `test_deleteNYCFC` | DELETE the &lt;NYCFC&gt; -> expects DELETED. |
    | 55 | `test_createEVL` | CREATE a &lt;EVL&gt; (EventLog) with `lgt`, `lgd`, `lgst` -> expects CREATED; checks `ri`. |
    | 56 | `test_retrieveEVL` | RETRIEVE the &lt;EVL&gt; -> expects OK; checks `mgd=EVL`. |
    | 57 | `test_attributesEVL` | RETRIEVE the &lt;EVL&gt; -> expects OK; checks `ty`, `pi`, `rn`, timestamps, `dc`, `lgt`, `lgd`, `lgst`, and `lga`/`lgo` (action flags) both true. |
    | 58 | `test_deleteEVL` | DELETE the &lt;EVL&gt; -> expects DELETED. |
    | 59 | `test_createWIFIC` | CREATE a &lt;WIFIC&gt; (wificlient) with `ssid`, `wcrds` (credentials dict: `enct`/`unm`/`pwd`), `scan=False` -> expects CREATED; checks `ri`. |
    | 60 | `test_retrieveWIFIC` | RETRIEVE the &lt;WIFIC&gt; -> expects OK; checks `mgd=WIFIC`. |
    | 61 | `test_attributesWIFIC` | RETRIEVE the &lt;WIFIC&gt; -> expects OK; checks `ty`, `pi`, `rn`, `ssid`, `wcrds.enct`/`unm`/`pwd`, `scan`, `scanr` (empty list), `ud`, `trdst`, `rdst` (all false defaults). |
    | 62 | `test_deleteWIFIC` | DELETE the &lt;WIFIC&gt; -> expects DELETED. |
    | 63 | `test_createWIFICCred1Fail` | CREATE a &lt;WIFIC&gt; with `wcrds.enct=2` combined with `unm`/`pwd` (invalid credential-type/field combination) -> expects BAD_REQUEST. |
    | 64 | `test_createWIFICCred2Fail` | CREATE a &lt;WIFIC&gt; with `wcrds.enct=4` combined with `wepk` (invalid combination) -> expects BAD_REQUEST. |
    | 65 | `test_createWIFICCred3Fail` | CREATE a &lt;WIFIC&gt; with `wcrds.enct=8` combined with `wpap` (invalid combination) -> expects BAD_REQUEST. |
    | 66 | `test_createDATC` | CREATE a &lt;DATC&gt; (dataCollection) with `cntp` (target container path) -> expects CREATED; checks `ri`. |
    | 67 | `test_updateDATCrpscIntegerFail` | UPDATE the &lt;DATC&gt; setting `rpsc` (report schedule) to a plain integer instead of a schedule-entry list -> expects BAD_REQUEST. |
    | 68 | `test_updateDATCmescIntegerFail` | UPDATE the &lt;DATC&gt; setting `mesc` (measurement schedule) to a plain integer -> expects BAD_REQUEST. |
    | 69 | `test_updateDATCrpscInvalidSchedule1Fail` | UPDATE `rpsc` with a malformed schedule entry (not a dict with `sce`) -> expects BAD_REQUEST. |
    | 70 | `test_updateDATCrpscInvalidSchedule2Fail` | UPDATE `rpsc` with a `sce` cron string that has the wrong number of fields (5 instead of 7) -> expects BAD_REQUEST. |
    | 71 | `test_updateDATCrpscValidSchedule` | UPDATE `rpsc` with a valid 7-field cron schedule -> expects UPDATED; checks `rpsc` is a list. |
    | 72 | `test_updateDATCmescInvalidSchedule1Fail` | UPDATE `mesc` with a malformed schedule entry -> expects BAD_REQUEST. |
    | 73 | `test_updateDATCmescInvalidSchedule2Fail` | UPDATE `mesc` with a wrongly-formatted cron string -> expects BAD_REQUEST. |
    | 74 | `test_updateDATCmescValidSchedule` | UPDATE `mesc` with a valid cron schedule -> expects UPDATED; checks `mesc` is a list. |
    | 75 | `test_updateDATCmeilWhileMescFail` | UPDATE the &lt;DATC&gt; setting `meil` (measurement interval) while `mesc` is already set (mutually exclusive) -> expects BAD_REQUEST. |
    | 76 | `test_updateDATCmescMeilFail` | UPDATE the &lt;DATC&gt; setting both `mesc` and `meil` together in one request -> expects BAD_REQUEST. |
    | 77 | `test_updateDATCremoveMescAddMeil` | UPDATE the &lt;DATC&gt; setting `mesc=None` and `meil=10000` together -> expects UPDATED; checks `mesc` absent and `meil` set. |
    | 78 | `test_attributesDATC` | RETRIEVE the &lt;DATC&gt; -> expects OK; checks `ty`, `pi`, `rn`, `cntp`, `mesc` absent, `meil=10000`, and `rpsc` has 1 entry with the expected `sce`. |
    | 79 | `test_deleteDATC` | DELETE the &lt;DATC&gt; -> expects DELETED. |
    | 80 | `test_createSIM` | CREATE a &lt;SIM&gt; with `imsi`, `icid`, `sist`, `sity`, `spn` -> expects CREATED; checks `mgd=SIM`. |
    | 81 | `test_retrieveSIM` | RETRIEVE the &lt;SIM&gt; -> expects OK; checks `mgd=SIM`. |
    | 82 | `test_attributesSIM` | RETRIEVE the &lt;SIM&gt; -> expects OK; checks `ty`, `pi`, `rn`, timestamps, `dc`, `imsi`, `icid`, `sist`, `sity`, `spn`. |
    | 83 | `test_deleteSIM` | DELETE the &lt;SIM&gt; -> expects DELETED. |
    | 84 | `test_createMNWK` | CREATE a &lt;MNWK&gt; (mobile network info) with `cnb`, `rss`, `liqu`, `ipad`/`ripa` (IP address lists), `apna`, `ceid`, `smnc`, `smcc`, `lac`, `coel` -> expects CREATED; checks `mgd=MNWK`. |
    | 85 | `test_retrieveMNWK` | RETRIEVE the &lt;MNWK&gt; -> expects OK; checks `mgd=MNWK`. |
    | 86 | `test_attributesMNWK` | RETRIEVE the &lt;MNWK&gt; -> expects OK; checks `ty`, `pi`, `rn`, timestamps, `dc`, and all the network attributes (`cnb`, `rss`, `liqu`, `ipad`/`ripa` contents, `apna`, `ceid`, `smnc`, `smcc`, `lac`, `coel`). |
    | 87 | `test_deleteMNWK` | DELETE the &lt;MNWK&gt; -> expects DELETED. |
    | 88 | `test_createCRDS` | CREATE a &lt;CRDS&gt; (credentials) with `pur`, `crid`, `crse`, `crtk` -> expects CREATED; checks `mgd=CRDS`. |
    | 89 | `test_retrieveCRDS` | RETRIEVE the &lt;CRDS&gt; -> expects OK; checks `mgd=CRDS`. |
    | 90 | `test_attributesCRDS` | RETRIEVE the &lt;CRDS&gt; -> expects OK; checks `ty`, `pi`, `rn`, timestamps, `dc`, `pur`, `crid`, `crse`, `crtk`. |
    | 91 | `test_deleteCRDS` | DELETE the &lt;CRDS&gt; -> expects DELETED. |

??? note "`testLocation.py` — Geo-query functionality and location-based queries (73 tests)"

    | # | Test Method | Requests Performed |
    |---|---|---|
    | 1 | `test_createContainerWrongLocFail` | CREATE a &lt;CNT&gt; with `loc` set to a malformed (non-dict) value -> expects BAD_REQUEST. |
    | 2 | `test_createContainerLocWrongAttributesFail` | CREATE a &lt;CNT&gt; with `loc` containing an unknown sub-attribute (`wrong`) -> expects BAD_REQUEST. |
    | 3 | `test_createContainerLocPointIntCoordinatesFail` | CREATE a &lt;CNT&gt; with `loc.typ=1` (Point) and integer (not float) coordinates -> expects BAD_REQUEST. |
    | 4 | `test_createContainerLocPointWrongCountFail` | CREATE a &lt;CNT&gt; with `loc.typ=1` but multiple coordinate pairs (Point must have exactly one) -> expects BAD_REQUEST. |
    | 5 | `test_createContainerLocPoint` | CREATE a &lt;CNT&gt; with a valid `loc.typ=1` Point -> expects CREATED, checks `loc.typ`/`crd`; DELETE -> expects DELETED. |
    | 6 | `test_createContainerLocLineStringWrongCountFail` | CREATE a &lt;CNT&gt; with `loc.typ=2` (LineString) but only 1 coordinate pair (needs &gt;=2) -> expects BAD_REQUEST. |
    | 7 | `test_createContainerLocLineString` | CREATE a &lt;CNT&gt; with a valid `loc.typ=2` LineString (2 points) -> expects CREATED, checks `loc.typ`/`crd`; DELETE -> expects DELETED. |
    | 8 | `test_createContainerLocPolygonWrongCountFail` | CREATE a &lt;CNT&gt; with `loc.typ=3` (Polygon) but only 1 coordinate -> expects BAD_REQUEST. |
    | 9 | `test_createContainerLocPolygonWrongFirstLastCoordinateFail` | CREATE a &lt;CNT&gt; with `loc.typ=3` whose first and last coordinates don't match (polygon must be closed) -> expects BAD_REQUEST. |
    | 10 | `test_createContainerLocPolygon` | CREATE a &lt;CNT&gt; with a valid closed `loc.typ=3` Polygon -> expects CREATED, checks `loc.typ`/`crd`; DELETE -> expects DELETED. |
    | 11 | `test_createContainerLocMultiPointWrongFail` | CREATE a &lt;CNT&gt; with `loc.typ=4` (MultiPoint) and a malformed `crd` -> expects BAD_REQUEST. |
    | 12 | `test_createContainerLocMultiPointWrongCountFail` | CREATE a &lt;CNT&gt; with `loc.typ=4` and an incorrectly-nested coordinate structure -> expects BAD_REQUEST. |
    | 13 | `test_createContainerLocMultiPoint` | CREATE a &lt;CNT&gt; with a valid `loc.typ=4` MultiPoint -> expects CREATED, checks `loc.typ`/`crd`; DELETE -> expects DELETED. |
    | 14 | `test_createContainerLocMultiLineStringWrongFail` | CREATE a &lt;CNT&gt; with `loc.typ=5` (MultiLineString) and a malformed `crd` -> expects BAD_REQUEST. |
    | 15 | `test_createContainerLocMultiLineString2WrongFail` | CREATE a &lt;CNT&gt; with `loc.typ=5` and an incorrectly-nested `crd` -> expects BAD_REQUEST. |
    | 16 | `test_createContainerLocMultiLineString` | CREATE a &lt;CNT&gt; with a valid `loc.typ=5` MultiLineString (2 lines) -> expects CREATED, checks `loc.typ`/`crd`; DELETE -> expects DELETED. |
    | 17 | `test_createContainerLocMultiPolygonWrongFail` | CREATE a &lt;CNT&gt; with `loc.typ=6` (MultiPolygon) and a malformed `crd` -> expects BAD_REQUEST. |
    | 18 | `test_createContainerLocMultiPolygonWrongFirstLastCoordinateFail` | CREATE a &lt;CNT&gt; with `loc.typ=6` whose ring isn't closed -> expects BAD_REQUEST. |
    | 19 | `test_createContainerLocMultiPolygon` | CREATE a &lt;CNT&gt; with a valid closed `loc.typ=6` MultiPolygon -> expects CREATED, checks `loc.typ`/`crd`; DELETE -> expects DELETED. |
    | 20 | `test_geoQueryGmtyOnlyFail` | RETRIEVE the &lt;AE&gt; with `rcn=4` and only `gmty` (missing `gsf`/`geom`) -> expects BAD_REQUEST. |
    | 21 | `test_geoQueryGeomOnlyFail` | RETRIEVE with `rcn=4` and only `geom` -> expects BAD_REQUEST. |
    | 22 | `test_geoQueryGsfOnlyFail` | RETRIEVE with `rcn=4` and only `gsf` -> expects BAD_REQUEST. |
    | 23 | `test_geoQueryGeomWrongFail` | RETRIEVE with `rcn=4`, `gmty=1`, `gsf=1`, and a malformed `geom` value -> expects BAD_REQUEST. |
    | 24 | `test_geoQueryPointWithinPolygon` | CREATE a &lt;CNT&gt; with a Polygon `loc`; RETRIEVE with a Point geo-query (`gsf=1` within) inside the polygon -> expects OK with a match; DELETE -> expects DELETED. |
    | 25 | `test_geoQueryPointOutsidePolygon` | Same Polygon &lt;CNT&gt;; geo-query Point outside it (`gsf=1`) -> expects OK with no match; DELETE -> expects DELETED. |
    | 26 | `test_geoQueryPointWithinPoint` | CREATE a &lt;CNT&gt; with a Point `loc`; geo-query the same Point (`gsf=1`) -> expects OK with a match; DELETE -> expects DELETED. |
    | 27 | `test_geoQueryPointContainsPoint` | Same Point &lt;CNT&gt;; geo-query the same Point with `gsf=2` (contains) -> expects OK with a match; DELETE -> expects DELETED. |
    | 28 | `test_geoQueryPointContainsPolygonFail` | CREATE a &lt;CNT&gt; with a Polygon `loc`; geo-query with `gsf=2` (contains) against a Point shape -> expects OK with no match (a polygon resource can't be 'contained' the wrong way round); DELETE -> expects DELETED. |
    | 29 | `test_geoQueryPointIntersectsPoint` | CREATE a Point &lt;CNT&gt;; geo-query the same Point with `gsf=3` (intersects) -> expects OK with a match; DELETE -> expects DELETED. |
    | 30 | `test_geoQueryPointIntersectsPointFail` | Same Point &lt;CNT&gt;; geo-query a different, non-intersecting Point (`gsf=3`) -> expects OK with no match; DELETE -> expects DELETED. |
    | 31 | `test_geoQueryPointIntersectsPolygon` | CREATE a Polygon &lt;CNT&gt;; geo-query with a Point inside it (`gsf=3`) -> expects OK with a match; DELETE -> expects DELETED. |
    | 32 | `test_geoQueryPointIntersectsPolygonFail` | Same Polygon &lt;CNT&gt;; geo-query with a Point outside it (`gsf=3`) -> expects OK with no match; DELETE -> expects DELETED. |
    | 33 | `test_geoQueryLineStringWithinPolygon` | CREATE a Polygon &lt;CNT&gt;; geo-query with a LineString fully inside it (`gsf=1`) -> expects OK with a match; DELETE -> expects DELETED. |
    | 34 | `test_geoQueryLineStringOutsidePolygon` | Same Polygon &lt;CNT&gt;; geo-query with a LineString fully outside it (`gsf=1`) -> expects OK with no match; DELETE -> expects DELETED. |
    | 35 | `test_geoQueryPointWithinLineString1` | CREATE a LineString &lt;CNT&gt;; geo-query with a Point on the line (`gsf=1`) -> expects OK with a match; DELETE -> expects DELETED. |
    | 36 | `test_geoQueryPointWithinLineString2` | Same LineString &lt;CNT&gt;; geo-query with a different on-line Point (`gsf=1`) -> expects OK with a match; DELETE -> expects DELETED. |
    | 37 | `test_geoQueryLineStringContainsLineString` | CREATE a LineString &lt;CNT&gt;; geo-query with `gsf=2` against a LineString that the resource's geometry contains -> expects OK with a match; DELETE -> expects DELETED. |
    | 38 | `test_geoQueryLineStringContainsPolygonFail` | CREATE a Polygon &lt;CNT&gt;; geo-query with `gsf=2` against a LineString shape -> expects OK with no match; DELETE -> expects DELETED. |
    | 39 | `test_geoQueryPointIntersectsLineString` | CREATE a LineString &lt;CNT&gt;; geo-query with `gsf=3` against a Point lying on the line -> expects OK with a match; DELETE -> expects DELETED. |
    | 40 | `test_geoQueryLineStringIntersectsLineString` | CREATE a LineString &lt;CNT&gt;; geo-query with `gsf=3` against an intersecting LineString -> expects OK with a match; DELETE -> expects DELETED. |
    | 41 | `test_geoQueryLineStringIntersectsLineStringFail` | Same LineString &lt;CNT&gt;; geo-query with `gsf=3` against a non-intersecting LineString -> expects OK with no match; DELETE -> expects DELETED. |
    | 42 | `test_geoQueryPolygonWithinPolygon` | CREATE a Polygon &lt;CNT&gt;; geo-query with `gsf=1` against a larger enclosing Polygon -> expects OK with a match; DELETE -> expects DELETED. |
    | 43 | `test_geoQueryPolygonOutsidePolygon` | Same Polygon &lt;CNT&gt;; geo-query with `gsf=1` against a non-overlapping Polygon -> expects OK with no match; DELETE -> expects DELETED. |
    | 44 | `test_geoQueryPolygonPartlyWithinPolygonFail` | CREATE a Polygon &lt;CNT&gt;; geo-query with `gsf=1` against a Polygon that only partially overlaps (not fully within) -> expects OK with no match; DELETE -> expects DELETED. |
    | 45 | `test_geoQueryPolygonContainsPolygon` | CREATE a larger Polygon &lt;CNT&gt;; geo-query with `gsf=2` against a smaller Polygon fully inside it -> expects OK with a match; DELETE -> expects DELETED. |
    | 46 | `test_geoQueryPolygonContainsPolygonFail` | Same Polygon &lt;CNT&gt;; geo-query with `gsf=2` against a Polygon outside it -> expects OK with no match; DELETE -> expects DELETED. |
    | 47 | `test_geoQueryPolygonIntersectsPolygon` | CREATE a Polygon &lt;CNT&gt;; geo-query with `gsf=3` against an overlapping Polygon -> expects OK with a match; DELETE -> expects DELETED. |
    | 48 | `test_geoQueryPolygonIntersectsPolygonFail` | Same Polygon &lt;CNT&gt;; geo-query with `gsf=3` against a non-overlapping Polygon -> expects OK with no match; DELETE -> expects DELETED. |
    | 49 | `test_geoQueryMultiPointWithinPolygon` | CREATE a MultiPoint &lt;CNT&gt;; geo-query with `gsf=1` against an enclosing Polygon shape -> expects OK with a match; DELETE -> expects DELETED. |
    | 50 | `test_geoQueryMultiPointOutsidePolygon` | Same MultiPoint &lt;CNT&gt;; geo-query with `gsf=1` against a non-enclosing Polygon -> expects OK with no match; DELETE -> expects DELETED. |
    | 51 | `test_geoQueryMultiPointOutsidePolygonWrongGmtyFail` | Same MultiPoint &lt;CNT&gt;; geo-query using a mismatched `gmty` (Polygon shape type) against an out-of-range query polygon -> expects OK with no match; DELETE -> expects DELETED. |
    | 52 | `test_geoQueryMultiPointContainsPoint` | CREATE a MultiPoint &lt;CNT&gt;; geo-query with `gsf=2` against one of its own points -> expects OK with a match; DELETE -> expects DELETED. |
    | 53 | `test_geoQueryMultiPointContainsPointFail` | Same MultiPoint &lt;CNT&gt;; geo-query with `gsf=2` against a Point not in the set -> expects OK with no match; DELETE -> expects DELETED. |
    | 54 | `test_geoQueryPointIntersectsMultiPoint` | CREATE a Point &lt;CNT&gt;; geo-query with `gsf=3` against a MultiPoint query shape that includes that point -> expects OK with a match; DELETE -> expects DELETED. |
    | 55 | `test_geoQueryPointIntersectsMultiPointFail` | Same Point &lt;CNT&gt;; geo-query with `gsf=3` against a MultiPoint shape that doesn't include it -> expects OK with no match; DELETE -> expects DELETED. |
    | 56 | `test_geoQueryMultiPointIntersectsMultiPoint` | CREATE a MultiPoint &lt;CNT&gt;; geo-query with `gsf=3` against an overlapping MultiPoint shape -> expects OK with a match; DELETE -> expects DELETED. |
    | 57 | `test_geoQueryMultiPointIntersectsMultiPointFail` | Same MultiPoint &lt;CNT&gt;; geo-query with `gsf=3` against a non-overlapping MultiPoint shape -> expects OK with no match; DELETE -> expects DELETED. |
    | 58 | `test_geoQueryMultiLinestringWithinPolygon` | CREATE a MultiLineString &lt;CNT&gt;; geo-query with `gsf=1` against an enclosing Polygon -> expects OK with a match; DELETE -> expects DELETED. |
    | 59 | `test_geoQueryMultiLinestringOutsidePolygon` | Same MultiLineString &lt;CNT&gt;; geo-query with `gsf=1` against a non-enclosing Polygon -> expects OK with no match; DELETE -> expects DELETED. |
    | 60 | `test_geoQueryMultiLineContainsPoint` | CREATE a MultiLineString &lt;CNT&gt;; geo-query with `gsf=2` against a Point on one of its lines -> expects OK with a match; DELETE -> expects DELETED. |
    | 61 | `test_geoQueryMultiLineContainsPointFail` | Same MultiLineString &lt;CNT&gt;; geo-query with `gsf=2` against a Point not on any line -> expects OK with no match; DELETE -> expects DELETED. |
    | 62 | `test_geoQueryPointIntersectsMultiLine` | CREATE a Point &lt;CNT&gt;; geo-query with `gsf=3` against a MultiLineString shape it lies on -> expects OK with a match; DELETE -> expects DELETED. |
    | 63 | `test_geoQueryPointIntersectsMultiLineFail` | Same Point &lt;CNT&gt;; geo-query with `gsf=3` against a MultiLineString it doesn't intersect -> expects OK with no match; DELETE -> expects DELETED. |
    | 64 | `test_geoQueryLineStringIntersectsMultiLine` | CREATE a LineString &lt;CNT&gt;; geo-query with `gsf=3` against an overlapping MultiLineString -> expects OK with a match; DELETE -> expects DELETED. |
    | 65 | `test_geoQueryLineStringIntersectsMultiLineFail` | Same LineString &lt;CNT&gt;; geo-query with `gsf=3` against a non-overlapping MultiLineString -> expects OK with no match; DELETE -> expects DELETED. |
    | 66 | `test_geoQueryMultiPolygonWithinPolygon` | CREATE a MultiPolygon &lt;CNT&gt;; geo-query with `gsf=1` against an enclosing Polygon -> expects OK with a match; DELETE -> expects DELETED. |
    | 67 | `test_geoQueryMultiPolygonOutsidePolygon` | Same MultiPolygon &lt;CNT&gt;; geo-query with `gsf=1` against a non-enclosing Polygon -> expects OK with no match; DELETE -> expects DELETED. |
    | 68 | `test_geoQueryMultiPolygonContainsPoint` | CREATE a MultiPolygon &lt;CNT&gt;; geo-query with `gsf=2` against a Point inside one of its polygons -> expects OK with a match; DELETE -> expects DELETED. |
    | 69 | `test_geoQueryMultiPolygonContainsPointFail` | Same MultiPolygon &lt;CNT&gt;; geo-query with `gsf=2` against a Point outside all polygons -> expects OK with no match; DELETE -> expects DELETED. |
    | 70 | `test_geoQueryPointIntersectsMultiPolygon` | CREATE a Point &lt;CNT&gt;; geo-query with `gsf=3` against a MultiPolygon shape containing that point -> expects OK with a match; DELETE -> expects DELETED. |
    | 71 | `test_geoQueryPointIntersectsMultiPolygonFail` | Same Point &lt;CNT&gt;; geo-query with `gsf=3` against a MultiPolygon that doesn't contain it -> expects OK with no match; DELETE -> expects DELETED. |
    | 72 | `test_geoQueryPolygonIntersectsMultiPolygon` | CREATE a MultiPolygon &lt;CNT&gt;; geo-query with `gsf=3` against an overlapping Polygon -> expects OK with a match; DELETE -> expects DELETED. |
    | 73 | `test_geoQueryPolygonIntersectsMultiPolygonFail` | Same MultiPolygon &lt;CNT&gt;; geo-query with `gsf=3` against a non-overlapping Polygon -> expects OK with no match; DELETE -> expects DELETED. |

??? note "`testLCP.py` — LocationPolicy (&lt;LCP&gt;) resource functionality (17 tests)"

    | # | Test Method | Requests Performed |
    |---|---|---|
    | 1 | `test_createLCPMissingLosFail` | CREATE an &lt;LCP&gt; under the &lt;AE&gt; without the mandatory `los` (location source) attribute -> expects BAD_REQUEST. |
    | 2 | `test_createMinimalLCP` | CREATE a minimal &lt;LCP&gt; with only `los=2` (device based) -> expects CREATED, checks `lost` (location status) is empty; DELETE the &lt;LCP&gt; -> expects DELETED. |
    | 3 | `test_createLCPWithSameCNTRnFail` | CREATE an &lt;LCP&gt; whose `lon` (location container name) equals its own resource name -> expects BAD_REQUEST. |
    | 4 | `test_createLCPWithLOS2LotFail` | CREATE an &lt;LCP&gt; with `los=2` (device based) together with `lot` (locationTargetID, invalid for that source) -> expects BAD_REQUEST. |
    | 5 | `test_createLCPWithLOS2AidFail` | CREATE an &lt;LCP&gt; with `los=2` together with `aid` (authID, invalid for that source) -> expects BAD_REQUEST. |
    | 6 | `test_createLCPWithLOS2LorFail` | CREATE an &lt;LCP&gt; with `los=2` together with `lor` (locationServer, invalid for that source) -> expects BAD_REQUEST. |
    | 7 | `test_createLCPWithLOS2RlklFail` | CREATE an &lt;LCP&gt; with `los=2` together with `rlkl=True` (retrieveLastKnownLocation, invalid for that source) -> expects BAD_REQUEST. |
    | 8 | `test_createLCPWithLOS2LuecFail` | CREATE an &lt;LCP&gt; with `los=2` together with `luec` (locationUpdateEventCriteria, invalid for that source) -> expects BAD_REQUEST. |
    | 9 | `test_createLCPWithWrongGtaFail` | CREATE an &lt;LCP&gt; with an invalid `gta` (geoTargetArea) value -> expects BAD_REQUEST. |
    | 10 | `test_createLCPWithGta` | CREATE an &lt;LCP&gt; with a valid GeoJSON polygon as `gta` -> expects CREATED, checks `gta` is present; DELETE the &lt;LCP&gt; -> expects DELETED. |
    | 11 | `test_createLCPWithLit2Lou0` | CREATE an &lt;LCP&gt; with `lit=2` (geo-fence) and `lou=['PT0S']` (zero update period) -> expects CREATED, checks `lost` is empty; DELETE the &lt;LCP&gt; -> expects DELETED. |
    | 12 | `test_testPeriodicUpdates` | CREATE an &lt;LCP&gt; geo-fence policy with `lou=PT1S` (periodic 1s updates), linked container, target polygon, and `gec=2` (leaving) -> expects CREATED; CREATE a &lt;CIN&gt; with an outside-the-polygon location -> expects CREATED; sleep; RETRIEVE the linked container's latest &lt;CIN&gt; -> expects OK, checks the geo-fence event code equals 2 (leaving); DELETE the &lt;LCP&gt; -> expects DELETED. |
    | 13 | `test_testManualUpdates` | CREATE an &lt;LCP&gt; geo-fence policy with no `lou` (manual updates) and `gec=2` -> expects CREATED; CREATE a &lt;CIN&gt; with an outside location -> expects CREATED; RETRIEVE the latest &lt;CIN&gt; -> expects OK, checks the geo-fence event equals 2 (leaving); DELETE the &lt;LCP&gt; -> expects DELETED. |
    | 14 | `test_testManualInsideEvent` | CREATE an &lt;LCP&gt; with `gec=3` (inside); feed a sequence of inside/outside &lt;CIN&gt; location updates (inside, inside, outside, outside, inside again) -> each CREATE expects CREATED; after each, RETRIEVE the latest &lt;CIN&gt; -> expects OK and checks the geo-fence event code matches the expected inside/non-inside transitions for each step; DELETE the &lt;LCP&gt; -> expects DELETED. |
    | 15 | `test_testManualOutsideEvent` | Same sequence of location updates as the inside-event test but with `gec=4` (outside) -> each CREATE/RETRIEVE checks the geo-fence event reflects outside/non-outside transitions correctly at each step; DELETE the &lt;LCP&gt; -> expects DELETED. |
    | 16 | `test_testManualLeavingEvent` | Same location-update sequence with `gec=2` (leaving) -> checks the leaving event fires only on the inside->outside transition and not otherwise; DELETE the &lt;LCP&gt; -> expects DELETED. |
    | 17 | `test_testManualEnteringEvent` | Same location-update sequence with `gec=1` (entering) -> checks the entering event fires only on the outside->inside transition and not otherwise; DELETE the &lt;LCP&gt; -> expects DELETED. |

??? note "`testSCH.py` — Schedule (&lt;SCH&gt;) resource functionality (22 tests)"

    | # | Test Method | Requests Performed |
    |---|---|---|
    | 1 | `test_createSCHunderCBwithNOCFail` | CREATE a &lt;SCH&gt; under &lt;CSEBase&gt; with `nco=True` (notification congestion, unsupported there) -> expects CONTENTS_UNACCEPTABLE. |
    | 2 | `test_createSCHunderNODwithNOCUnsupportedFail` | CREATE a &lt;SCH&gt; under &lt;NOD&gt; with `nco=True` (unsupported parent type for that feature) -> expects NOT_IMPLEMENTED. |
    | 3 | `test_createSCHunderCBwithoutNCO` | CREATE a &lt;SCH&gt; under &lt;CSEBase&gt; without `nco`, with a schedule expression -> expects CREATED; DELETE it -> expects DELETED. |
    | 4 | `test_updateSCHunderCBwithNCOFail` | CREATE a &lt;SCH&gt; under &lt;CSEBase&gt; -> expects CREATED; UPDATE it adding `nco=True` -> expects CONTENTS_UNACCEPTABLE; DELETE it -> expects DELETED. |
    | 5 | `test_updateSCHunderNODwithNOCUnsupportedFail` | CREATE a &lt;SCH&gt; under &lt;NOD&gt; -> expects CREATED; UPDATE it adding `nco=True` -> expects NOT_IMPLEMENTED; DELETE it -> expects DELETED. |
    | 6 | `test_createSCHunderSUBwrongRn` | CREATE a &lt;SUB&gt; -> expects CREATED; CREATE a &lt;SCH&gt; under it with an incorrect, non-standard resource name -> expects BAD_REQUEST; DELETE the &lt;SUB&gt; -> expects DELETED. |
    | 7 | `test_createSCHunderSUBemptyRn` | CREATE a &lt;SUB&gt; -> expects CREATED; CREATE a &lt;SCH&gt; under it with no `rn` -> expects CREATED (default name applied); DELETE the &lt;SUB&gt; -> expects DELETED. |
    | 8 | `test_createSCHunderSUBcorrectRn` | CREATE a &lt;SUB&gt; -> expects CREATED; CREATE a &lt;SCH&gt; under it with the correct fixed name `notificationSchedule` -> expects CREATED; DELETE the &lt;SUB&gt; -> expects DELETED. |
    | 9 | `test_createSCHunderCRSwrongRn` | CREATE a &lt;SCH&gt; under the pre-existing &lt;CRS&gt; with an incorrect resource name -> expects BAD_REQUEST. |
    | 10 | `test_createSCHunderCRSemptyRn` | CREATE a &lt;SCH&gt; under the &lt;CRS&gt; with no `rn` -> expects CREATED; DELETE it -> expects DELETED. |
    | 11 | `test_createSCHunderCRScorrectRn` | CREATE a &lt;SCH&gt; under the &lt;CRS&gt; with the correct fixed name -> expects CREATED; DELETE it -> expects DELETED. |
    | 12 | `test_createSCHunderCB` | CREATE a &lt;SCH&gt; directly under &lt;CSEBase&gt; -> expects CREATED; DELETE it -> expects DELETED. |
    | 13 | `test_createSCHunderCBTwiceFail` | CREATE a &lt;SCH&gt; under &lt;CSEBase&gt; -> expects CREATED; CREATE a 2nd one under the same parent -> expects BAD_REQUEST (only one schedule allowed); DELETE the first -> expects DELETED. |
    | 14 | `test_createSCHunderNOD` | CREATE a &lt;SCH&gt; under &lt;NOD&gt; -> expects CREATED; DELETE it -> expects DELETED. |
    | 15 | `test_testSCHunderSUBinsideSchedule` | CREATE a &lt;SUB&gt; -> expects CREATED; CREATE a &lt;SCH&gt; under it with a schedule window starting soon -> expects CREATED; UPDATE the &lt;AE&gt; to trigger a notification -> expects UPDATED; checks a notification IS received once inside the scheduled window; DELETE the &lt;SUB&gt; -> expects DELETED. |
    | 16 | `test_testSCHunderSUBoutsideSchedule` | Same setup but the schedule window starts later (outside the immediate check window) -> UPDATE the &lt;AE&gt; -> expects UPDATED; checks NO notification is received yet; DELETE the &lt;SUB&gt; -> expects DELETED. |
    | 17 | `test_testSCHunderSUBoutsideScheduleImmediate` | Same out-of-window schedule but the &lt;SUB&gt; has `nec=2` (immediate notification, bypassing schedule) -> UPDATE the &lt;AE&gt; -> expects UPDATED; checks a notification IS received immediately despite being outside the schedule; DELETE the &lt;SUB&gt; -> expects DELETED. |
    | 18 | `test_testSCHunderSUBWithOMinsideSchedule` | CREATE a &lt;SUB&gt; with an `om` (operationMonitor) on RETRIEVE -> expects CREATED; CREATE a &lt;SCH&gt; with an immediate window -> expects CREATED; RETRIEVE the &lt;AE&gt; (the monitored operation) -> expects OK; checks a notification is received inside the schedule; DELETE the &lt;SUB&gt; -> expects DELETED. |
    | 19 | `test_testSCHunderSUBWithOMoutsideSchedule` | Same operationMonitor &lt;SUB&gt; but with a later schedule window -> RETRIEVE the &lt;AE&gt; -> expects OK; checks NO notification yet; DELETE the &lt;SUB&gt; -> expects DELETED. |
    | 20 | `test_testSCHunderSUBWithOMoutsideScheduleImmediate` | Same as above but `nec=2` (immediate) on the &lt;SUB&gt; -> RETRIEVE the &lt;AE&gt; -> expects OK; checks a notification IS received immediately despite the later schedule; DELETE the &lt;SUB&gt; -> expects DELETED. |
    | 21 | `test_testSCHunderCRSinsideSchedule` | CREATE a &lt;CRS&gt; (cross-resource subscription monitoring the &lt;AE&gt;) -> expects CREATED; CREATE a &lt;SCH&gt; under it with an immediate window -> expects CREATED; UPDATE the &lt;AE&gt; to trigger evaluation -> expects UPDATED; checks a notification IS received inside the window; DELETE the &lt;CRS&gt; -> expects DELETED. |
    | 22 | `test_testSCHunderCRSoutsideScheduleFail` | Same &lt;CRS&gt; setup but the &lt;SCH&gt; window starts later -> UPDATE the &lt;AE&gt; -> expects UPDATED; checks NO notification is received (outside the schedule); DELETE the &lt;CRS&gt; -> expects DELETED. |

??? note "`testPDR.py` — PolicyDeletionRules resource functionality (5 tests)"

    | # | Test Method | Requests Performed |
    |---|---|---|
    | 1 | `test_createPDR` | CREATE a &lt;PDR&gt; under the &lt;NTP&gt; (created in setUp) -> expects CREATED. |
    | 2 | `test_retrievePDR` | RETRIEVE the &lt;PDR&gt; -> expects OK. |
    | 3 | `test_updatePDR` | UPDATE the &lt;PDR&gt; setting `lbl` -> expects UPDATED; checks the returned `lbl` matches. |
    | 4 | `test_deletePDR` | DELETE the &lt;PDR&gt; -> expects DELETED. |
    | 5 | `test_createTooManyPDRsFail` | CREATE 2 additional &lt;PDR&gt;s under the &lt;NTP&gt; in a loop -> each expects CREATED; CREATE a 3rd &lt;PDR&gt; -> expects CONFLICT (exceeds the maximum allowed PDRs per NTP); DELETE the 2 created &lt;PDR&gt;s -> each expects DELETED. |

??? note "`testSMD.py` — Semantic Descriptor (&lt;SMD&gt;) resource functionality (13 tests)"

    | # | Test Method | Requests Performed |
    |---|---|---|
    | 1 | `test_createSMDdcrpIRIFail` | CREATE a &lt;SMD&gt; under the &lt;AE&gt; with `dcrp` (descriptor representation) set to the IRI value -> expects BAD_REQUEST. |
    | 2 | `test_createSMDdspNotBase64Fail` | CREATE a &lt;SMD&gt; with `dsp` (descriptor) set to a non-base64 string -> expects BAD_REQUEST. |
    | 3 | `test_createSMDdspBase64` | CREATE a &lt;SMD&gt; with `dcrp=4` and a base64-encoded RDF/XML `dsp` -> expects CREATED. |
    | 4 | `test_deleteSMD` | DELETE the &lt;SMD&gt; -> expects DELETED. |
    | 5 | `test_createSMDunderACPFail` | CREATE an &lt;ACP&gt; under the &lt;AE&gt; -> expects CREATED; attempt to CREATE a &lt;SMD&gt; under that &lt;ACP&gt; -> expects INVALID_CHILD_RESOURCE_TYPE; DELETE the &lt;ACP&gt; -> expects DELETED. |
    | 6 | `test_updateSMDwithSOEandDSPFail` | UPDATE the &lt;SMD&gt; setting both `soe` and `dsp` simultaneously -> expects BAD_REQUEST (mutually exclusive attributes). |
    | 7 | `test_updateSMDwithVLDEtrue` | UPDATE the &lt;SMD&gt; setting `vlde=True` (validation enable) -> expects UPDATED; checks the returned `vlde` is `True`. |
    | 8 | `test_updateSMDwithVLDEfalse` | UPDATE the &lt;SMD&gt; setting `vlde=False` -> expects UPDATED; checks `vlde` is `False` and `svd` (semantic validation descriptor result) is also falsy. |
    | 9 | `test_semanticQueryOnlyRCNFail` | RETRIEVE the &lt;AE&gt; with only `rcn=semanticContent` (no semantic query format) -> expects BAD_REQUEST. |
    | 10 | `test_semanticQueryOnlySQIFail` | RETRIEVE the &lt;AE&gt; with only `sqi=true` (no query content) -> expects BAD_REQUEST. |
    | 11 | `test_semanticQueryOnlySMF` | RETRIEVE the &lt;AE&gt; with only `smf` (semantic filter/query) set, a SPARQL query -> expects OK. |
    | 12 | `test_semanticQueryAsDiscoveryFail` | RETRIEVE combining `fu=1` (discovery), `sqi=true`, `rcn=semanticContent`, and `smf` together -> expects BAD_REQUEST (semantic query cannot be combined with discovery). |
    | 13 | `test_semanticQuery` | RETRIEVE the &lt;AE&gt; with `sqi=true`, `rcn=semanticContent`, and a SPARQL `smf` query -> expects OK; checks the `m2m:qres` result is well-formed XML/SPARQL-results or JSON output. |
