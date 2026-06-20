# Test Reference

*Part of [Unit Tests](../UnitTests.md) — see that page for configuration and how to run the test suites.*

This section catalogs all unit tests in the [`tests/`](https://github.com/ankraft/ACME-oneM2M-CSE/tree/master/tests) directory of the ACME oneM2M CSE repository (files matching `test*.py`). For each test, the **Requests Performed** column describes the actual CRUD/NOTIFY operations, target resources, and expected response codes exercised — derived directly from the test source, not just the docstring.

**42 test modules, 1077 individual test cases**, grouped below by functional area.

| Area | Modules | Tests |
|---|---|---|
| [Core CSE & Registration](core-cse.md) | 5 | 81 |
| [Resource Tree: Containers & Data](containers-data.md) | 8 | 190 |
| [Access Control & Security](access-control.md) | 2 | 72 |
| [Subscriptions & Notifications](subscriptions.md) | 5 | 202 |
| [Groups & Actions](groups-actions.md) | 3 | 80 |
| [Discovery, Requests & Expiration](discovery-requests.md) | 4 | 100 |
| [Management, Location, Scheduling & Policies](management-location.md) | 6 | 221 |
| [Polling Channels](polling-channels.md) | 2 | 26 |
| [Remote CSE / Inter-CSE](remote-cse.md) | 4 | 54 |
| [Load & Miscellaneous](load-misc.md) | 3 | 51 |
| **Total** | **42** | **1077** |
