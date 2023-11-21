# Remove Booking from the Timetable System

Have you accidentally added something? Has an event been cancelled or changed? Well, this guide is for you!

## 🧠 Background

The timetable displays are Raspberry Pi's which display [*yggdrasil2*](https://github.com/SoCSTech/yggdrasil-revamp) which pulls its data from *Mimir* which is the timetabling system from the [*Asgard* Stack](https://github.com/SoCSTech/asgard-system-stack).

## 📝 Before You Start

Before you start you should have access to the webserver via SSH, and know the database password. 

If you do not have this - ask another technician who will set you up.

## 🗑️ Deleting an event

1. **Connect to the web server via SSH.**

2. **Run the following command to connect to the Docker Container.**

```sh
docker exec -it asgard-db mysql -u asgard -p
```
Once you are connected, you should have a sql shell. You will know this because the line should start with `mysql>`.

3. **Use the Asgard Database**

You need to "use" the database, this means we don't have to ask for it with every following request.

```sql
use asgardDB;
```

4. **Find the timetable event** 

Let's select all the event's that are in our table and output them to the screen (you may need to make your terminal bigger).

```sql
select * from mimir_bookings;
```

When you've found which one needs to be deleted, you should select only that one with a `WHERE` clause.

```sql
select * from mimir_bookings where identifier = 5131;
```

You should only see one result, and this should be for the event you want to remove.

![SQL response with just one item](/images/SelectMyEvent.png)

5. **Deleting that event**

You want to take your time while deleting an event to make sure you have the commands correct. You should `select` what you are deleting first, to ensure you've only got the one you are deleting.

Once you are happy you want to change the command from `select *` to * `delete`.

```sql
delete from mimir_bookings where identifier = 5131;
```

⚠️ If you don't use the `WHERE` clause, you will delete all events. So double check before you execute the command.

This should have deleted the event. You should see a response of:

> Query OK, 1 row affected (0.04 sec)

Once you see this - you have deleted the event.
