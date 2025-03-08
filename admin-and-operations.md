# Admin and Operations

### Admin Roles

There are three admin account that hold the admin privilege of the smart contract. Both admin accounts permissioned MSafe account that ensures security.

* **Admin**: Ultimate controller / root admin.
* **PoolAdmin**: Pool related configuration & Emergency pause & Collect protocol fees
* **Rewarder**: Distribute and manage reward.

#### **1.1. Admin**

**Description**

Ultimate admin of the smart contract , not expecting to operate very often.

**Responsibilities**

* Rewarder & PoolAdmin role assign
* Contract upgrade
* Toggle trading & Enable fee rate

#### 1.2.PoolAdmin

**Description**

A operator role assigned by Admin that will constantly operate on the pool config

**Responsibilities**

* All contract configuration (protocol fee, swap fee, loan fee rate…)
* Collect protocol fees
* Emergency stop
* Misc
  * Increase observation cardinality next

**1.3. Rewarder**

**Description**

A operator role assigned by Admin that will constantly operate on the reward distribution.

**Responsibility**

* Distribute the reward
* Update the rewarder settings

### Admin addresses

All admin addresses are secured by MSafe, multi-sig solution on Sui to reduce the single point failure.&#x20;

| Role            |                                                                    |
| --------------- | ------------------------------------------------------------------ |
| admin-msafe     | 0x396a933434a340a8932f5a644b2d3a3d8a50f9297f2165eb9127219a06a741f6 |
| poolAdmin-msafe | 0xbe6faaf95e723873a5f4b34dc2ff51d25c739f618a39f53fd9a71b3d201c7aa8 |
| rewarder-msafe  | 0x506ecadb1d93eb2f9e7e1d32e5146b60d734f6d02bd763e8ec705ba00eaded30 |
