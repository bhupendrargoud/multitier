# To install Redis server on Ubuntu, you can follow these steps:

1. Update your Ubuntu system's package manager by running the following command:

```
sudo apt-get update
```

2. Install Redis server by running the following command:

```
sudo apt-get install redis-server
```

3. After the installation is complete, you can start the Redis server by running the following command:

```
sudo systemctl start redis-server
```

4. To check if the Redis server is running, you can run the following command:

```
redis-cli ping
```

If the Redis server is running, it will return "PONG".

5. If you want Redis to start automatically at boot time, you can enable the service by running the following command:

```
sudo systemctl enable redis-server
```

After completing these steps, you should have Redis server installed and running on your Ubuntu system.

# To install Python with Flask plugin server on Ubuntu, you can follow these steps:

1. Install Python by running the following command:

```
sudo apt-get install python3
```

2. Install pip, which is the package installer for Python, by running the following command:

```
sudo apt-get install python3-pip
```

3. Install the Redis Python library by running the following command:

```
sudo pip3 install redis
sudo pip3 install flask
```
