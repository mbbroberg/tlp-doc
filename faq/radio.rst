Radio Devices: Bluetooth, Wi-Fi, WWAN
=====================================

Slow or unstable Wi-Fi on battery power
---------------------------------------
Cause: kernel driver does not implement power saving properly.

Solution: disable power saving on battery ::

    WIFI_PWR_ON_BAT=off

Wi-Fi power saving is activated despite being disabled in TLP's configuration
-----------------------------------------------------------------------------
Cause: conflict with `NetworkManager`.

Solution: remove the file **/etc/NetworkManager/conf.d/default-wifi-powersave-on.conf**.

.. _faq-bluetooth-unstable:

Bluetooth devices hang, disconnect or do not pair
-------------------------------------------------
Cause: most internal laptop bluetooth devices and all external bluetooth
dongles are USB devices. Some do not implement autosuspend mode properly,
giving trouble to connected devices or preventing discovery or pairing.

.. rubric:: Solutions

Version 1.0 and newer: configure :ref:`set-usb-blacklist-btusb` =1.

Version 0.9 and before: identify the USB bluetooth device with ::

    tlp-stat -u | grep btusb

and add it to :ref:`set-usb-blacklist`.

Apply the new setting with ::

    sudo tlp usb

Bluetooth is not disabled upon system startup
---------------------------------------------
Probable cause: your desktop environment's bluetooth applet – or some other
installed software – re-enables the bluetooth device after TLP disabled it.

Solution: disable the relevant applet's setting or disable/remove the causing
applet. For XFCE/blueman see
`Disable Bluetooth Auto Power-on in Blueman <https://winaero.com/blog/disable-bluetooth-auto-power-blueman/>`_.

Radio states are not restored according to `RESTORE_DEVICE_STATE_ON_STARTUP=1`
------------------------------------------------------------------------------
Cause: conflict with other settings, for instance :ref:`set-radio-disable-on` ff.

Solution: don't use `RESTORE_DEVICE_STATE_ON_STARTUP=1` and
:ref:`set-radio-disable-on` ff. simultaneously.

Cause: systemd implements its own radio state restore scheme
`systemd-rfkill.service`

Solution: use either `RESTORE_DEVICE_STATE_ON_STARTUP=1` or systemd's approach
but not both.
