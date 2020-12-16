.. highlight:: console

.. author:: Julius Künzel

.. tag:: web
.. tag:: privacy 

.. sidebar:: Logo

  .. image:: _static/images/keycloak.svg 
      :align: center

####
Keycloak
####

.. tag_list::

Keycloak is an open source identity and access management system which allows single sign-on for web applications and services.

----

.. note:: For this guide you should be familiar with the basic concepts of

  * :manual:`supervisord <daemons-supervisord>`
  * :manual:`Backends <web-backends>`

License
=======

Keycloak is released under the `Apache 2.0 license`_.

Prerequisites
=============

Your URL needs to be setup:

.. include:: includes/web-domain-list.rst

Installation
============

Download Keycloak
------

Check the Keycloak_ website or `Github Repository`_ for the latest release and copy the download link to the .tar.gz file. Then use ``wget`` to download it. Replace the URL with the one you just got from GitHub.

::

 [isabell@stardust ~]$ wget https://downloads.jboss.org/keycloak/12.0.0/keycloak-12.0.0.tar.gz
 […]
 Saving to: ‘keycloak-12.0.0.tar.gz’

 100%[======================================>] 7,750,708   6.64MB/s   in 1.1s

 2020-12-16 16:56:27 (6.64 MB/s) - ‘keycloak-12.0.0.tar.gz’ saved [7750708/7750708]
 [isabell@stardust ~]$

Extract the archive
------

Use ``tar`` to extract the archive and delete the archive. Replace the version in the archive file name with the one you just downloaded.

::

 [isabell@stardust ~]$ tar -xvzf keycloak-12.0.0.tar.gz keycloack
 keycloak
 [isabell@stardust ~]$

You can now delete the archive:

::

 [isabell@stardust ~]$ rm keycloak-12.0.0.tar.gz
 [isabell@stardust ~]$

Configuration
============

Change the configuration
------

Find the following block in file ``standalone/configuration/standalone.xml`` and add the ``proxy-address-forwarding="true"`` attribute to ``<http-listener>`` element under <server>.

.. code-block:: xml
 :emphasize-lines: 4

 <subsystem xmlns="urn:jboss:domain:undertow:10.0" default-server="default-server" default-virtual-host="default-host" default-servlet-container="default" default-security-domain="other" statistics-enabled="${wildfly.undertow.statistics-enabled:${wildfly.statistics-enabled:false}}">
     <buffer-cache name="default"/>
     <server name="default-server">
         <http-listener name="default" socket-binding="http" redirect-socket="https" enable-http2="true" read-timeout="30000" proxy-address-forwarding="true"/>
         <https-listener name="https" socket-binding="https" security-realm="ApplicationRealm" enable-http2="true" read-timeout="30000"/>
         <host name="default-host" alias="localhost">
             <location name="/" handler="welcome-content"/>
             <http-invoker security-realm="ApplicationRealm"/>
         </host>
     </server>
     ...
 </subsystem>


Create an admin user
------

Replace ``<username>`` with a user name of your choice and enter a password when your a asked to

::
 :emphasize-lines: 2

 [isabell@stardust ~]$ cd ~/keycloak/bin
 [isabell@stardust keycloak]$ ./add-user-keycloak.sh -u <username>
 Press ctrl-d (Unix) or ctrl-z (Windows) to exit
 Password: 
 [isabell@stardust keycloak]$

Setup daemon
------

Use your favourite editor to create the file ~/etc/services.d/keycloack.ini with the following content. Replace the version in the archive file name with the one you just downloaded.

::
 :emphasize-lines: 2

 [program:keycloak]
 command=/home/smartlab/keycloak/keycloak-12.0.0/bin/standalone.sh -b 0.0.0.0
 autostart=yes
 autorestart=yes

.. include:: includes/supervisord.rst



Setup Backend
------

.. note::

    Keycloak is running on port 8080.

.. include:: includes/web-backend.rst


Finishing installation
======================

Point your Browser to your installation URL ``https://isabell.uber.space`` and use Keycloak!

Tuning
======

For further information on configuration and usage go to https://www.keycloak.org/documentation.

Updates
=======

.. note:: Check the update feed_ regularly to stay informed about the newest version.


.. _Keycloak: https://www.keycloak.org/downloads
.. _feed: https://github.com/keycloak/keycloak/releases.atom
.. _Apache 2.0 License: https://github.com/gohugoio/hugo/blob/master/LICENSE
.. _Github Repository: https://github.com/keycloak/keycloak/releases

----

Tested with Keycloak 12.0.0, Uberspace 7.8.0

.. author_list::
