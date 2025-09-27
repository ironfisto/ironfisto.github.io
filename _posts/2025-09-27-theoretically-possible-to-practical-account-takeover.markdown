---
layout: post
title: "Theoretically Possible to Practical Account Takeover"
date: 2025-09-27 00:00:00 +0000
categories: jekyll update
---

The article describes an account takeover vulnerability found in a crypto mining web application. Initially, the vulnerability seemed theoretical:

1.  **Initial Discovery:** The author found that the password reset link used a `user-primary-key-id` and a `reset-token` (e.g., `https://domain.tld/changePassword/{user-primary-key-id}/{reset-token}`). By replacing their own `user-primary-key-id` with a victim's, while keeping their own `reset-token`, they could reset the victim's password. However, this required knowing the victim's primary key (UUID).

2.  **Making it Practical:** The author then discovered a way to obtain a victim's UUID. The application had a referral functionality that generated links like `https://domain.tld/?source=4vyryrtfhf`. A separate endpoint, `https://domain.tld/fast/{source_parameter}`, when accessed with the victim's referral source parameter, leaked the victim's UUID. Using Google dorks, the author could find many source parameters.

3.  **Account Takeover:** With the victim's UUID, the attacker could then use their own generated reset token in conjunction with the victim's UUID to successfully reset the victim's password and take over their account.

The impact was gaining access to the victim's crypto assets information, and the author was rewarded a fraction of ETH.
