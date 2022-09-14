# HADynamicWeather
Use GPS messages for dynamic weather and location in Home Assistant in your RV (Pepwave example)

I have a Pepwave router, and it can send GPS (GPMRC format amongst others) to another system using the "GPS Forwarding" feature.  Since I wanted to get this into Home Assistant, I configured the Pepwave router to send it to Home Assistant on UDP port 10110.
Well, HA does not listen on port 10110, so we use NodeRed to open up that port and listen for the incoming messages.  When one is received, it takes the message, splits it up, and publishes it to MQTT.  In the same Node Red flow, there is the option to create all the entities you need with a push of a button through MQTT Discovery.

Once you do this, you will have a number of entities in Home Assistant that you can use for all sorts of things - see screenshot below.
Now, with the dynamic coordinates coming into Home Assistant, we can use them in the Lovelace dashboard.  Since Lovelace doesn't like dynamic information (other than displaying state), we use the "custom:config-template-card" to dynamically change the contents of an iframe whenever the GPS coordinates change (sample dash view below).

The neat thing is, we can use this capability to change the views whenever we want by adding a button that pushes any GPS coordinates to change the view (check on the weather at Grandma's house before you head over, etc)
OK, this is a bit long, but hopefully it helps someone.
