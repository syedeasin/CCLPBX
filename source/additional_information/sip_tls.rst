*****************
SSL/TLS Setup
*****************

On a new installation of cclpbx, TLS for SIP is available to use once you run `letsencrypt.sh <../getting_started/lets_encrypt.html>`_ and make a few setting changes in cclpbx.


Configure TLS
^^^^^^^^^^^^^^^

Configuration for SIP to use TLS can be achieved with the following steps.

* First open an ssh terminal or console window.

* cd /usr/src/cclpbx-install.sh/debian/resources/

* Execute `letsencrypt.sh <../getting_started/lets_encrypt.html>`_

* Login to your cclpbx installation.

* Go to Advanced > Variables.

* Scroll down to **SIP Profile:** Internal (This can be done on any SIP Profile)

.. image:: ../_static/images/cclpbx_switch_tls.jpg
        :scale: 85%



* Set **internal_ssl_enable** value to **true** in lowercase.

* Go to Status > SIP Status.

* Click **FLUSH CACHE** at the top right.


.. image:: ../_static/images/cclpbx_tls_sofia_status2.jpg
        :scale: 85%


* Click **Rescan** on the profile.



.. image:: ../_static/images/cclpbx_tls_sofia_status1.jpg
        :scale: 85%


* You should now see at the right under **State** (RUNNING)(0)(TLS)

.. image:: ../_static/images/cclpbx_tls_sofia_status.jpg
        :scale: 85%










