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
