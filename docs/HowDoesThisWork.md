# How does this work?

## Let's start with the goals of this project:
* Have the driver station software report that an FMS is connected.
* Be able to complete a "match" without having to buy $20,000 worth of equipment.

## And what was _not_ a goal:
* Award assignments
* Match schedule generation
* Connecting to any field electronics (I expect this to be used on a practice field with no electronics, meaning no stack lights or displays or anything on the field)
* Force teams to do anything weird to connect to it. (I didn't want to force network cables to the robot or something like that.)
* Any sense of security provided by the field network. (It's just complex and not required for a practice field, also managed switches are expensive and complex)

## Here is how FIRST made this easier for me:
In the home version of the radio configuration utility, there is what is called "FMS Offseason" mode. This has the robot radios connected automatically to a given network. This means I don't have to deal with how to connect radios to the network. This took a major hurdle away since I only had to deal with the driver station to/from FMS communication.
Also, all robot driving is directly from driver station to robot, so I don't need to interfere with their communication, I just need to make sure that they can connect to each other.

## Here is how Team 254, the Cheesy Poofs, made this easier for me:
In case you didn't know, the Cheesy Poofs host an annual event called the Cheesy Champs, and they use their own custom FMS called Cheesy Arena.
[They post their source code publicly on GitHub](https://github.com/Team254/Cheesy-Arena), which means there is open source information on what is required to connect to a driver station and tell it that it is connected to an FMS.

## How does it work?
When you open the program, you are prompted to enter a team number for each alliance station. For each team number, it is converted into two different IP addresses (10.TE.AM.1 for the robot radio and 10.TE.AM.2 for the RoboRio), and we save that information. We use that infomation to do things like ping the robot radio and RoboRio to check that they are still connected before and during a match.

After all the team numbers have been entered, we start waiting for driver stations to communicate with us. Since the FMS server IP is hardcoded, 10.0.100.5, the driver stations know how to reach the FMS. The driver stations will send a [packet of information](https://github.com/ORF-4450/PracticeFMS/blob/eb711122961ce81e93aee656db2f40d6dc7a0ade/PFMS/Main.cs#L341-L353) requesting to be in the match. If that team is in the match, we [save the IP address of the driver station](https://github.com/ORF-4450/PracticeFMS/blob/eb711122961ce81e93aee656db2f40d6dc7a0ade/PFMS/DriverStation.cs#L94-L99) and [tell the driver station it is ready to go](https://github.com/ORF-4450/PracticeFMS/blob/eb711122961ce81e93aee656db2f40d6dc7a0ade/PFMS/Main.cs#L375-L380). We also start [sending control packets](https://github.com/ORF-4450/PracticeFMS/blob/eb711122961ce81e93aee656db2f40d6dc7a0ade/PFMS/DriverStation.cs#L116-L186) with information like match info and match timers.

Once the driver stations start receiving control packets, they enter FMS mode (Indicated by the blue bar under the robot status and the "FMS Connected" message appearing where the enable/disable buttons are).

At this point, the PracticeFMS program is checking for three things from each team:
- There is a driver station for the team.
- The program can ping the robot's radio.
- The program can ping the RoboRio.

Once those 3 conditions are met for every team, the match can be started. When the countdown is started, we send and initial [Game Specific String packet](https://github.com/ORF-4450/PracticeFMS/blob/eb711122961ce81e93aee656db2f40d6dc7a0ade/PFMS/DriverStation.cs#L188-L208) to the driver stations to reset their game strings. For the 2020 season, we send a empty string until a criteria is met during the match. (Replicated here as a button push intead of an automated system)

Now that the match has started, we track the current match phase (Autonomous, Teleop, etc.) and how much time is left in each match phase. Every 100ms, we [send an updated control packet](https://github.com/ORF-4450/PracticeFMS/blob/eb711122961ce81e93aee656db2f40d6dc7a0ade/PFMS/DriverStation.cs#L116-L186) to the driver stations with the updated match phase and time remaining.

Also happening when the match starts is creating the [E-stop thread](https://github.com/ORF-4450/PracticeFMS/blob/eb711122961ce81e93aee656db2f40d6dc7a0ade/PFMS/Main.cs#L77-L113). This thread runs the entire time there are robots enabled and allows the PracticeFMS program to E-stop any robot or the entire match. E-stopping a robot is as simple as adding a byte to the control packet, and the way we E-stop a match by E-stopping every robot and forcing the timer to 0.

[Return Home](index.md)