.. |labmodule| replace:: 1
.. |labnum| replace:: 1
.. |labdot| replace:: |labmodule|\ .\ |labnum|
.. |labund| replace:: |labmodule|\ _\ |labnum|
.. |labname| replace:: Lab\ |labdot|
.. |labnameund| replace:: Lab\ |labund|

Lab |labmodule|\.\ |labnum|\: Install the Ubuntu Base
-----------------------------------------------------

In this lab you will walk through installing the ubuntu
base ready for ELK stack

Task 1 - GIT Clone Repo onto the Server
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

.. code::

git clone https://github.com/jarrodlucia/bigip_elk_server <directory of choice>


Task 2 - Install additional software required for ELK Stack
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

.. code::
	
  sudo apt-get install software-properties-common
  sudo apt install curl


Task 3 - Add and install Java
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

.. code::

  sudo add-apt-repository -y ppa:webupd8team/java
  sudo apt-get update
  sudo apt-get -y install oracle-java8-installer

**Accept the Oracle License Agreement**

|oracle|

.. |oracle| image:: /_static/oracle.png
   :width: 7.0in
   :height: 4.0in


**Fix Below for Java8 Error (If Required)**

.. code-block:: none

   sudo apt-get -y install oracle-java8-installer
   sudo sed -i 's|JAVA_VERSION=8u144|JAVA_VERSION=8u152|' oracle-java8-installer.*
   sudo sed -i 's|PARTNER_URL=http://download.oracle.com/otn-pub/java/jdk/8u144-b01/090f390dda5b47b9b721c7dfaa008135/|PARTNER_URL=http://download.oracle.com/otn-pub/java/jdk/8u152-b16/aa0333dd3019491ca4f6ddbe78cdb6d0/|' oracle-java8-installer.*
   sudo sed -i 's|SHA256SUM_TGZ="e8a341ce566f32c3d06f6d0f0eeea9a0f434f538d22af949ae58bc86f2eeaae4"|SHA256SUM_TGZ="218b3b340c3f6d05d940b817d0270dfe0cfd657a636bad074dcabe0c111961bf"|' oracle-java8-installer.*
   sudo sed -i 's|J_DIR=jdk1.8.0_144|J_DIR=jdk1.8.0_152|' oracle-java8-installer.*
   sudo apt-get -y install oracle-java8-installer