.. |labmodule| replace:: 1
.. |labnum| replace:: 4
.. |labdot| replace:: |labmodule|\ .\ |labnum|
.. |labund| replace:: |labmodule|\ _\ |labnum|
.. |labname| replace:: Lab\ |labdot|
.. |labnameund| replace:: Lab\ |labund|

Lab |labmodule|\.\ |labnum|\: Install Logstash
----------------------------------------------

Install Logstash

Task 1 - Install Logstah
^^^^^^^^^^^^^^^^^^^^^^^^

#. Install Logstash

.. code::

  sudo apt-get install logstash


#. Install Additional Plugins

.. code::

  sudo /usr/share/logstash/bin/logstash-plugin install logstash-filter-dns
  sudo /usr/share/logstash/bin/logstash-plugin install logstash-filter-geoip

.. NOTE:: 
  
  **Be patient with plugin install it can take a few moments**


|logstash1|


.. |logstash1| image:: /_static/logstash1.png
   :width: 12.0in
   :height: 3.0in


#. Copy or Create new file to Directory /etc/logstash/conf.d/

.. code::

  sudo cp <git clone directory>/config_files/logstash.conf /etc/logstash/conf.d/logstash.conf
  sudo vi /etc/logstash/conf.d/logstash.conf


#. Logstash restart

.. code::

  sudo systemctl restart logstash.service


#. Check logstash started correctly with no errors from logstash.conf file


|logstash2|


.. |logstash2| image:: /_static/logstash2.png
   :width: 12.0in
   :height: 3.0in


#. To configure Logstash to start automatically when the system boots up, run the following commands:
   
.. code::

  sudo /bin/systemctl daemon-reload
  sudo /bin/systemctl enable logstash.service


#. Logstash Control

.. code::

  sudo systemctl start logstash.service
  sudo systemctl stop logstash.service
  sudo systemctl status logstash.service


**logstash.conf**

   .. code-block:: json
      :linenos:
      :emphasize-lines: 3,7,11

        input {
            tcp {
                port => 5516
                type => afm
            }
            tcp {
                port => 5515
                type => dns
            }
            tcp {
                port => 5514
                type => pem
            }
        }

        filter {
            if [type] == 'pem' {
                kv {
                  source => "message"
                 field_split => ","
               }
            }
            if [type] == 'afm' {
                kv {
                  source => "message"
                  field_split => ","
              }
                geoip {
                    source => "SourceIp"
                    target => "SourceIp_geo"
                    add_field => [ "[geoip][coordinates]", "%{[geoip][longitude]}" ]
                    add_field => [ "[geoip][coordinates]", "%{[geoip][latitude]}"  ]
                }
                geoip {
                    source => "DestinationIp"
                    target => "DestinationIp_geo"
                    add_field => [ "[geoip][coordinates]", "%{[geoip][longitude]}" ]
                    add_field => [ "[geoip][coordinates]", "%{[geoip][latitude]}"  ]
                }
                mutate {
                    convert => [ "[geoip][coordinates]", "float"]
                }   
            } 
            if [type] == 'dns' {
                kv {
                  source => "message"
                  field_split => ","
              }
            }
        }

        output {
            if [type] == 'pem' {
              elasticsearch {
              hosts => ["10.1.1.5:9200"]
              index => "pem-%{+YYYY.MM.dd}"
              template_name => "pem"
            }
            }
            if [type] == 'afm' {
              elasticsearch {
              hosts => ["10.1.1.5:9200"]
              index => "afm-%{+YYYY.MM.dd}"
              template_name => "afm"
            }
            }
            if [type] == 'dns' {
              elasticsearch {
              hosts => ["10.1.1.5:9200"]
              index => "dns-%{+YYYY.MM.dd}"
              template_name => "dns"
            }
            }
            stdout {}
        }


