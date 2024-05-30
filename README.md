# SSH into your Linux machine over the internet FOR FREE

## Usage:

***What you'll only have to do once***

1- You'll need to create a **free** ngrok account (and get your authtoken, which you'll need for creating TCP tunnels)

2- You'll need to create a **free** HiveMQ account (and get your user name, and password, which you'll need to connect to the broker)

3- Modify ```mqtt_communicator.py``` with your HiveMQ account info

    # Hive MQ account info
    HIVE_MQ_USER, PASSWD = "USERNAME", "PASSWORD"
    HIVE_MQ_URL, PORT = "YOUR_HIVE_MQ_URL", 8883

4- Modify ```server.py``` with your ngrok authtoken, and the SSH port on your machine

    TUNNEL, PORT = "tcp", "YOUR_SSH_PORT"
    NGROK_AUTH_TOKEN = "PUT_YOUR_AUTH_TOKEN_HERE" # Make a free account and get this

> By default, your SSH port will be 22, please change it to something like 6969, [tutorial](https://www.interserver.net/tips/kb/change-ssh-port-ubuntu/)

***What you'll only have to do each time you want a connection***

On the Linux machine that you want to SSH into, run ```python3 server.py```, this script will launch the ngrok SSH tunnel, send the URL:PORT over MQTT, kill the MQTT connection, and keep the SSH tunnel.

And on the other machine, run ```python3 consumer.py```, this script will connect to the MQTT broker, read the last retained data (URL:PORT), save it to a file named ```addr_port_ngrok.txt```, then die.

Now using the data you just retrieved from HiveMQ (the broker in general), ```ssh [USER]@[URL] -p [PORT]```

