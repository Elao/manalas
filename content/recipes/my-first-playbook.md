---
type:               "howto"
title:              "My first playbook"
date:               "2015-07-21"
publishdate:        "2015-07-21"
draft:              false
slug:               "my-first-playbook"
description:        "This how-to will present how we decide to organize our playbooks"
---

There's many way to organize your roles depending on your needs and your habit.
We changed many times our typical organisation to finaly keep one of them. 
Event if this one is still moving, it's quite stable and will allow you to avoid futur organizational problems.

No surprise in there, our structure is based on default arborescence suggest by Ansible.

We will start by creating in our working directory call `ansible` (Wow such imagination in naming things ... ) our first file `playbook.yml`.

Our tree should like as following:

```
ansible
└── playbook.yml
```

