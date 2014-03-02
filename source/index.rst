Bananaphone codec and Pluggable Transport's documentation
==========================================================


Bananaphone Pluggable Transport
================================

.. code-block:: none

  $ obfsproxy bananaphone
  usage: obfsproxy bananaphone [-h] [--corpus CORPUS]
                             [--encoding_spec ENCODINGSPEC]
                             [--model MODELNAME] [--order ORDER] [--abridged]
                             [--dest DEST] [--ext-cookie-file EXT_COOKIE_FILE]
                             {server,ext_server,client,socks} listen_addr

encoding_spec: string containing comma seperated values consisting of a tokenizer, hash function and bits per token. For instance: words,sha1,2

corpus: the text file to be used as a data source to generate the model

model: currently only "markov" and "random" (random weighted) models are implemented

order: number of previous tokens which constitute a previous state

abridged: if set to "true" then all states in the model which do not lead to complete hash spaces will be removed



Testing the bananaphone obfsproxy transport
--------------------------------------------


bananaphone-client-torrc file:

.. code-block:: none
  :emphasize-lines: 7,8

  Log notice stdout
  SocksPort 8040
  DataDirectory ./client-data
 
  UseBridges 1
 
  Bridge bananaphone 127.0.0.1:4703 modelName=markov corpus=/usr/share/dict/words encodingSpec=words,sha1,4 order=1
  ClientTransportPlugin bananaphone exec /usr/local/bin/obfsproxy --log-min-severity=info --log-file=/var/log/tor/obfsproxy-logs/obfsproxy-client.log managed



bananaphone-bridge-torrc file:

.. code-block:: none
  :emphasize-lines: 10,11,12

  Log notice stdout
  SocksPort 0
  ORPort 7001
  ExitPolicy reject *:*
  DataDirectory ./bridge-data
 
  BridgeRelay 1
  PublishServerDescriptor 0
 
  ServerTransportListenAddr bananaphone 127.0.0.1:4703
  ServerTransportPlugin bananaphone exec /usr/local/bin/obfsproxy --log-min-severity=info --log-file=/var/log/tor/obfsproxy-logs/obfsproxy-bridge.log managed
  ServerTransportOptions bananaphone corpus=/usr/share/dict/words encodingSpec=words,sha1,4 modelName=markov order=1





Bananaphone codec
=================


.. toctree::
   :maxdepth: 2



Indices and tables
==================

* :ref:`genindex`
* :ref:`modindex`
* :ref:`search`

