# Prescriptive-Cardio

Did Prescriptive Cardio Live up to the Hype?

Based on my initial outline of this project, unfortunately Prescriptive Cardio did not live up to the hype due to incompatibility issues and networking errors, but I learned a lot about IoT technologies and truly enjoyed working on the project along the way.

a. What did I implement instead?

I implemented a Swift application that simulates recordings of heart rate readings, and uses MQTT to publish these recordings to my local machine in a Node-Red flow. At this point, Node-Red, parses and stores the readings in a MariaDB stored on AWS. Next, the most recent reading from the database is selected and used as a parameter to validate what the user should do. Lastly, the results are published back to a Terminal window for the user to see.

c. What lessons did I learn?

I learned that if you want to undertake scoping a project to this size, it will take time. I spent about 2-3 weeks working on this project and probably needed another week to meet the requirements I initially set out to do by properly debugging each of the errors I ran into. In the future, I would either scope out a smaller project, or expect to spend more time to successfully meet the expected results.

c1. What did I learn?

MQTT:

I learned how to use SwiftMQTT, an MQTT Client that works on iOS devices. I particularly learned how to use establishConnection and publish functions that came with the package.

Node-Red:

I gained experience using Node-Red, particularly using the Switch nodes to conditionally change the flow of a current data stream and use the countdown node from the node-red-contrib-countdown package. I also worked with connecting, inserting data and retrieving data from the MariaDB that I hosted on AWS.

MQTT Troubles:

Socket Error in Swift: Initially, I was expecting to use both the publish and subscribe functions on the phone to simulate what a user would see on their watch, but came to realize that after publishing one message to the phone from another device, that the app would report: nw_socket_handle_socket_event [C1.1:1] Socket SO_ERROR [54: Connection reset by peer] swift. This resulted in a Stream and Socket Error on the user interface which prevented any further publishing or subscribe messages to flow from the simulator. After looking into this for some time with no success, I decided that I could no longer use the simulator to display feedback to the user.
Failure to Connect to AWS EC2 Instance: I spun up an EC2 instance over the weekend expecting to host Node-Red and MQTT there for the final results. After successfully setting up the instance, I was unable to properly input the credentials to Node-Red which this [tutorial](https://www.fis.gatech.edu/how-to-configure-nodered-mqtt-publishing-for-aws-iot-core/) outlines. Looking elsewhere, I followed this [tutorial](https://www.youtube.com/watch?v=t1r8GFVd6b0&t=99s&ab_channel=benphan) where I was able to set up an "IoT Thing," but this only let me set up an MQTT out node from Node-Red.
Failure to Connect to External Device: Because I was unable to connect to the Watch, (which I outline why below), I decided I could show the MQTT results on another device, my Palm Phone. After installing an MQTT client on the Android device, I was unable to locate the credentials for my MQTT broker and tried the default user, password and host, but was unable to gain a successful connection to the broker from the device.

My Naivety of the SwiftMQTT Client:

While I did learn how to use the SwiftMQTT Client on iOS, I was unable to run it on watchOS and was naive to expect that if I could run it in iOS that it could also run on watchOS. The problem is that the SwiftMQTT package is not supported on watchOS, nor is any MQTT package supported on watchOS. The alternative would have been to use WCSession to receive the messages to the Phone and then publish them to Node-Red from there. I felt that this was out of scope of the project considering it is not a true IoT connection and would have required more time.

Node-Red Troubles:

I was able to conditionally output data based on what the user's current heart rate is; however, there are only two conditions: for the user to run faster, or that the user is running at a High Intensity Pace. When the user stops running at a High Intensity Pace, the flow will immediately continue to display to the user that they should run faster. Gimme a break! Literally.

I had trouble using the countdown function for this feature precisely. I was hoping that after the user has completed a sprint, that it could enable a timer that would output true (indicating that the user just sprinted and display to the user that they should Cool Down), until the timer was up, and once the timer was up, it would return false, indicating to the user that they should run faster to reach the High Intensity Interval and repeat the cycle. I played around with the Status and Switch Nodes for some time, but finally threw in the towel after spending a couple hours and having some prototype that I could show.

d. How Could I Have Approached this Differently for Better Success?

Either I would have had to limit the scope to either working on just getting the data from the Watch published to Node-Red or working on creating an extensive algorithm that can validate and provide feedback to users during their run. Because the scope was large, I think that neither part of this project was developed enough as my time was split between working on both parts. In addition, I wanted to host everything on the cloud. While that would have been a great addition, it was unnecessary for the prototype and further consumed my time.

Something that I originally wanted to do and am still interested in is using are the ML nodes in Node-Red to further provide feedback to the user. This article, [Is This App the Future of AI Workouts?](https://www.outsideonline.com/health/training-performance/app-future-ai-workouts/) describes TrainerRoad's algorithm, as having the ability to "hunt through massive troves of data and suss out esoteric patterns that are invisible to the human brain." The author details the software as, "evaluating [his] performance and progress, and comparing [him] to everyone else on the platform ... this data is then used to prescribe future workouts- ranging from slow and steady endurance work to high-intensity sprint intervals - that are tailored just for [him]." CEO of the company expects that in 20 years, everyone will have their workouts picked by an AI.


I am a true believer of AI improving physical fitness technology. While we will see more medical applications of AI, it's important to realize that physical health is just as important. I think that many diseases can be prevented at the root, so while we should definitely be investing in technology to improve medical health solutions, let's not forget about physical health which can prevent many diseases and illnesses. Instead of a doctor prescribing medication, he could prescribe that a user follows a workout routine that is specific to a user which has numerous secondary benefits.
