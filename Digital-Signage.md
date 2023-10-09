
# Digital Signage

## Overview

We use a couple of systems to enable the usage of our signage dotted around the building; Balena (Dockerised magicness) & Anthias. These deal with actually showing the Timetabling and Content, not manage them. I'll explaing more shortly.

Before we continue - please make sure you are set up on our Balena account. Balena enables us to control the RPis remotely, and push new system software to them!

## Asgard (Room Timetabling)

Asgard is the System that produces our timetables for us. It can be a little bit awkward but it does the job. Due to a fair amount of scripting, it isn't really necessary to interact with it directly, rather, we just use the command line to really speed up the process of pushing out our timetables. For now, the timetable needs to be updated once weekly.

### Updating the timetable

We first need to grab the data for the timetable, which is slightly awkward due to some limitations imposed on us by ~~ICT~~ DT. As they don't have an easily accessible endpoint, we have to do this bit by hand, and then use some Python in order to turn that data into something that makes sense. 

First, go to [the University's timetabling system](https://timetable.lincoln.ac.uk), and just log into it. Once done, we can go directly to the timetables. We do this by dropping this into the address bar: `https://timetable.lincoln.ac.uk/roomtimetable/23-24/room/{room}`. Replace {room} with the room we want the timetable for. You should now see a huge text file, this is a JSON file and it contains the data used for the Timetable. If you are using Firefox, you should be able to just click "Copy" and it will all be put into your clipboard, else, just select it all and Copy manually.

Now we need to convert that into data that we can actually use. [The script is hosted here](https://replit.com/join/bdifhpfhpo-grooben), on Replit. Open the JSON folder, click the room you are updating, and simply replace the contents of that file with the new contents. Repeat these steps for the rooms you wish to update. Once ready, we can run the script. We do this by first clicking "Run" (it's the big green button at the top), and let it do its thing. Once it does the first run, we need to open the Shell. If you don't have a Shell tab within Replit, simply click the plus above the editor, and type Shell. In this tab, type `./big.sh` - this runs the script for all the Computing Labs, and creates an SQL file in the dump folder called `big.sql`, open this file and copy it's contents.

You will now need to connect to the server hosting all this via SSH. SSH will give you a terminal prompt in which we can interact with the **container** running the show. The IPs and your account details for these servers will be given to you by Ollie, Matt or whoever else is running this place. Once you're in, run this command `docker exec -it asgard-db mysql -u asgard -p`, again, the password it asks for will be given to you.  Once you're at the MySQL prompt, type `use asgardDB;` (remember the semicolon!), once selected - you can simply paste that massive set of SQL Queries into the prompt and it will update the timetables for you! Room displays check for new data very frequently, so by the time you walk from your desk to the nearest display, it will have already updated - unless it's the big timetable that is for all the computing labs, that differs mildly as you will need to restart the Pi (or wait up to 6 hours) for it to update. You can do this through Balena which will be explained shortly.
