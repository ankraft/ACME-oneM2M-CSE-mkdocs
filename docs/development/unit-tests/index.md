# Unit Test Reference

This section catalogs all unit tests in the [`tests/`](https://github.com/ankraft/ACME-oneM2M-CSE/tree/master/tests) directory of the ACME oneM2M CSE repository (files matching `test*.py`). For each test, the **Requests Performed** column describes the actual CRUD/NOTIFY operations, target resources, and expected response codes exercised — derived directly from the test source, not just the docstring.

**43 test modules, 1134 individual test cases**, grouped below by functional area.

| Area | Modules | Tests |
|---|---|---|
| [Core CSE & Registration](core-cse.md) | 4 | 70 |
| [Resource Tree: Containers & Data](containers-data.md) | 9 | 211 |
| [Access Control & Security](access-control.md) | 2 | 72 |
| [Subscriptions & Notifications](subscriptions.md) | 5 | 203 |
| [Groups & Actions](groups-actions.md) | 3 | 80 |
| [Discovery, Requests & Expiration](discovery-requests.md) | 4 | 108 |
| [Management, Location, Scheduling & Policies](management-location.md) | 7 | 249 |
| [Polling Channels](polling-channels.md) | 2 | 30 |
| [Remote CSE / Inter-CSE](remote-cse.md) | 4 | 54 |
| [Load & Miscellaneous](load-misc.md) | 3 | 57 |
| **Total** | **43** | **1134** |
