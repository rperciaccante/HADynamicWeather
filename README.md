# HADynamicWeather
Use GPS messages for dynamic weather and location in Home Assistant in your RV (Pepwave example)

I have a Pepwave router in my RV, and it can send GPS information (GPMRC format amongst others) to another system using the "GPS Forwarding" feature.  Since I wanted to get this into Home Assistant, I configured the Pepwave router to send it to Home Assistant on UDP port 10110.

HA does not listen on port 10110, so we use NodeRed to open up that port and listen for the incoming messages.  When one is received, it takes the message, splits it up, and publishes it to MQTT.  In the same Node Red flow, there is the option to create all the entities you need with a push of a button through MQTT Discovery.

Once you do this, you will have a number of entities in Home Assistant that you can use for automations, etc just as you would any other decive_tracker.
Since we now have dynamic coordinates coming into Home Assistant, we can use them in the Lovelace dashboard.  Since Lovelace doesn't like dynamic information (other than displaying state), we use the "custom:config-template-card" to dynamically change the contents of an iframe whenever the GPS coordinates change (sample dash view).

The neat thing is, we can use this capability to change the views whenever we want by adding a button that pushes any GPS coordinates to change the view (check on the weather at Grandma's house before you head over, etc)

(more) Detailed Instructions:

This requires that you have Home Assistant installed onto an appropriate device, and that you have access to Node-Red and a MQTT server.  These can be through Home Assistant if you are running Home Assistant Supervised, or they can run on a different server.  You will need to adapt the configuration accordingly.  You will also need to configure Home Assistant to communicate with the MQTT server (go to Settings -> Devices and Services -> Integrations and click "Add Integration" in the lower right).  Make sure that MQTT Discovery is enabled, and that the default "homeassistant/" discovery topic is selected.

In the Node-Red dashboard, import the contents of the file "format_gpmrc_messages.json" through the options menu in the upper right of Node-Red screen.  Once imported, edit the node "Build Pepwave GPS Sensor" and update the necessary information on the top (model, serial number, software version, etc).  Also, configure the mqtt node with the correct host info, depending on your MQTT server configuration. Deploy the changes with the "Deploy" option in the upper right.

Once Node-Red is configured, click on the button on the left of the "timestamp" node, and this will create the necessary items in Home Assistant to be ready to accept the GPS information. You can confirm this by going in Home Assistant to Settings -> Devices and Services -> Integrations and clicking on the device list.  Once confirmed, you can the move over to your Pepwave router.

Once you log into the Pepwave router, navigate to Advanced -> GPS Forwarding.  In this screen, enter the IP address of the Node-Red server, and leave the port as 10110.  As for frequency, I would leave it at 5 seconds while you are testing, then change it to 3600 (once an hour).  Once you enable this, click "save" and then "Apply Changes".  Confirm that the information is flowing by looking at the device under the MQTT Integration.

In order to display the weather information, you will need a custom component called "custom:config-template-card".  This can be installed using HACS (Home Assistant Community Store).  If you have not installed it yet, follow these directions here: https://hacs.xyz/docs/setup/download/  Once installed, search for, and install, the "custom:config-template-card".

Once "custom:config-template-card" is installed, go to your Lovelace dashboard, and add a new Manual card.  Erase everything in the input area by default, and copy the contents of "lovelace-custom-config-template-card" into that  block.  You should see the preview on the right, before you save it.

Now, this is based on the Windy website, and custom URL's can be created at www.windy.com to display the information you are most interested in.  Other services can be added to the dashboard using the same code, simply put in the new URL and replace the GPS coordinates with the LAT and LON variables.

Hope this helps!
