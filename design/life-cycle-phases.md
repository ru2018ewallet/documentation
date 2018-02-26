[something]: blabla, means that blabla is only available after [something]. Or to put it in other words: If [something] then MAYBE blabla but nobody enforces blabla.


Life Cycle and Methods
======================

OS completed
------------

Java card has an operating system, no applet is installed yet

The next phase *Initialization* is reached by installing our applet.

Initialiized (pre-personalized)
-------------------------------

The applet is installed, but not personalized.

The next phase is reached via *personalization*.

Available methods in this phase:

* enter_management_mode
 * authenticate_for_management_mode

* [management mode]: personalize
* [management mode]: drop_rights

Personalized
------------

After personalization the applet is ready to use.

The next phase *end of life* is reached via *termination*.

Available methods in this phase:

* temporary_lock_card (!TODO: do we want this?)
* authenticate_terminal_chip
* [auth TC]: authenticate_chip_terminal
* [auth TC]: establish_secure_channel
* authenticate_user_chip_(pin)
 * [authenticated]: get_user_info
 * [authenticated]: get_transaction_history
 * [authenticated]: get_curent_balance
 * [authenticated]: charge_balance
 * [authenticated]: pay_balance
* authenticate_for_management_mode
 * [management mode]: get_transaction_history
 * [management mode]: get_curent_balance
 * [management mode]: termination

End of Life
-----------

Key material is deleted.
It is made sure that the personalization got removed.
The card can not be re-issued.