Design Document - JavaCard eWallet
==================================


Description
-----------

We provide an applet for a JavaCard which provides
functionality of an (electronic)  wallet. A user can
put money in the wallet and use it to pay. Money is stored
on the card, according to design requirements.


Scope
-----

If not stated otherwise only the security of the Applet on
the Card is in scope. Security of the terminal is not in
scope of this document.


Stake holder and roles
----------------------

1. payment:: provider (we)
2. customer:: 'user'
3. payee:: the one who gets paid by the customer

other parties, e.g., card manufacturer, ..., are ignored.

Last but not least two additional roles (but not stake
holders) are introduced: anybody (unauthenticated user)
and an adversary


Assets
------

1. balance:: the amount of (electronic) money on the card
2. cryptographic material stored on the card

Crptographic material stored on the terminals is ignored


Life Cycle
----------

We define the following phases:

1. OS-completed
2. pre-personalization
3. personalized
4. end-of-life

However the state of OS-completion is out of scope.
See diagram ```life-cycle-phases.dot```


State Machine
-------------

The following basic states are defined and bound to a
phase in the lifecycle.

0. empty
1. pre
2. use
3. die
4. lock
5. mgmt?

where *empty* means, that we get an empty JavaCard (so OS
completion already took place), *pre* means the
pre-personalization phase where the applet is installed
but not personalized, *use* means that the applet got
persoanlized/bound to a user and is ready for use, *die*
means that no further usage is possible.

In addition we define multiple management phases *mgmt?*
and a (soft) *lock* state where the card can not be used
further, but can be unlocked again.

See diagram ```life-cycle-phases.dot```


Use Cases (ordered by state)
----------------------------

In the mentioned state the following actions can be
performed by the specified role.

0. empty  
 * technically everything is possible
 * payment provider (we) can install applet and lock the card (obviously we wont do this, but we would)
1. pre  
 * payment provider can get info: version
 * payment provider can switch to *mgmt1* state
2. mgmt1  
 * payment provider can switch back to *pre* state
 * payment provider can initialize personalization process
3. use  
 * user can charge/add money
 * user can pay/withdraw money
 * user can see current balance
 * user can see last 10 payments
 * user can change pin
 * user can switch to *die* state
 * payment provider can switch to *mgmt2* state
 * anybody can switch to *lock* state
4. mgmt2  
 * payment provider can switch back to *use* state
 * payment provider can read last 10 payments
 * payment provider can switch to *lock* state
 * payment provider can get info: version, user
 * payment provider can switch to *die* state
5. lock  
 * payment provider can switch back to *use* state
 * payment provider can get info: version, user, lock
6. die  
 * payment provider can get info: version, die

No other interactions should be possible.


Attacker model
--------------

Multiple adversaries must be considered:
1. adversaries evesdropping on the communication to get
knowledge about the balance or the user
2. adversaries actively taking part in the communication
to get knowledge about the balance or the user
3. adversaries who manipulate the card to receive
information about user, balance or cryptographic material.

We assume no attacker can interact the card before the
card reaches the *pre-personalization* phase. However
security of this phase is not in scope of this document.
Main focus is on the *personalized* and *end-of-life* stage