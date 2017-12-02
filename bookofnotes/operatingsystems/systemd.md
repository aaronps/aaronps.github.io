# SystemD

* if the **service** has autorecovery code, for example, you connecto to a JMS
server and you reconnect when connection drops. **USE `Wants` INSTEAD OF
`Requires`**

* If you want to start following some order, use `After=` or `Before=`