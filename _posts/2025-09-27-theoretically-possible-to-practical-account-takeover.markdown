---
layout: post
title: "Theoretically Possible to Practical Account Takeover"
date: 2025-09-27 00:00:00 +0000
categories: jekyll update
---

Hey InfoSec Community,

This is the last account takeover I found and wanted to share. It started as a nice chain of IDOR with some recon and understanding of the application.

---

#### Summary

* Target: a crypto-mining web app developed by a solo Russian developer.
* At first glance the app looked secure (withdrawals required identity verification).
* Impact (practical): disclosure of users' crypto balances and complete account takeover **if** you can obtain a victim’s UUID and a reset token.
* I moved the issue from theoretically possible → practical by enumerating referral/source parameters and mapping them to UUIDs.

---

#### Initial observation

* Accounts use a UUID as the primary key.
* Password reset link format observed:

```
https://domain.tld/changePassword/{user-primary-key-id}/{reset-token}
```

Because the primary key (UUID) was present in the URL, that immediately looked fishy.

---

#### PoC — high level

1. Create two accounts (attacker and attacker-2) and note their UUID primary keys.
2. Use the password reset flow for your account and grab the reset link containing your UUID + token.
3. Replace your UUID in the reset link with the victim’s UUID while keeping your reset token.
4. POST a new password to the changed URL and you can set the victim’s password.

---

#### Example request (PoC)

**Request**

```
POST /changePassword/62c4ffb0-be57-11e8-a68e-8d01686939c8/378fce77... HTTP/1.1
Host: domain.tld
Content-Type: application/json
```

**Request Body**

```json
{"password":"urhacked"}
```

**Result**

* Response: `200 OK`
* Outcome: Able to reset the victim’s password and take over the account.

> Note: initially this is a *theoretical* attack because you need the victim’s email or UUID. My first tests were attacker accounts targeting each other.

---

#### How I obtained victim UUIDs

* The app had referral links that looked like:

```
https://domain.tld/?source=4vyryrtfhf
```

* The `source` value alone did not leak the UUID.
* I discovered another endpoint that accepted the `source` param:

```
https://domain.tld/fast/4vyryrtfhf
```

*(image / screenshot placeholder — shows endpoint returning the victim UUID)*

* That endpoint returned the victim’s UUID.
* With Google dorks I enumerated many `source` values, fetched the `/fast/{source}` endpoint, got UUIDs, and then used my password-reset token with those UUIDs to takeover accounts.

---

#### Impact

* Full account takeover of any user whose `source` parameter can be enumerated and resolved to a UUID.
* Exposure of crypto balances and ability to perform account actions depending on what the app allowed post-login.
* Practical exploit path: enumerate `source` → map `source` → get UUID → use your reset token + victim UUID to change victim password.

---

#### Reward

* Received a fraction of ETH as a reward.

---

#### Closing

That’s a quick write-up from theoretical to practical account takeover for this app.
Bye.

---
Read more at [the original article](https://ironfisto.medium.com/theoretically-possible-to-practical-account-takeover-c9383ab03f76).
