## This lab would involve below AWS services.
## VPC, EC2, SG, NACL, VPC-Peering, EBS, Snapshot, AMI, IAM

![image](https://github.com/Vishwanathms/aws-saa-c03-scenarios/assets/19227977/da7301d4-12c1-43b3-9072-a4f8128f3c4b)
![image](https://github.com/Vishwanathms/aws-saa-c03-scenarios/assets/19227977/95f40b24-5968-492e-b68d-6e86a6485bc6)
![image](https://github.com/Vishwanathms/aws-saa-c03-scenarios/assets/19227977/21a1065c-eda9-4094-a640-6c610b426e10)



## Setting up a Redis-backed Counter with Flask and Python on the VM-1 and VM-2

### Introduction

This tutorial will guide you through the process of setting up a Redis-backed counter using Flask and Python. The tutorial assumes that you have a basic understanding of Python, Flask, and Redis.

### Prerequisites

- An AWS account
- Basic understanding of AWS EC2
- Basic understanding of Redis
- Basic understanding of Python and Flask

### Steps to setup Frontend server

1. Launch an Ubuntu EC2 instance (VM-1)  with ports 80, & 90 open
2. Install Apache2, Python, and the Redis Python library -- Refer "frontend-app-readiness.md
3. Set up the frontend HTML with the below code, to be placed in File Path: /var/www/html/index.html.
```
sudo vi /var/www/html/index.html
```

#### Frontend script:

```html
<!DOCTYPE html>
<html>
<head>
    <title>Redis Test</title>
</head>
<body>
    <h1>Redis Test</h1>
    <p>Current Count: <span id="count"></span></p>
    <button id="incrBtn">Increase</button>
    <button id="decrBtn">Decrease</button>
    <script>
    const countElem = document.getElementById("count");
    const incrBtn = document.getElementById("incrBtn");
    const decrBtn = document.getElementById("decrBtn");

    function updateCount() {
        fetch("http://Public-IP-of-frontend-app:90/get")
        .then(response => response.json())
        .then(data => {
            countElem.innerText = data.count;
        });
    }

    function increaseCount() {
        fetch("http://Public-IP-of-frontend-app:90/incr", {method: 'POST'})
        .then(() => updateCount());
    }

    function decreaseCount() {
        fetch("http://Public-IP-of-frontend-app:90/decr", {method: 'POST'})
        .then(() => updateCount());
    }

    updateCount();

    incrBtn.addEventListener("click", increaseCount);
    decrBtn.addEventListener("click", decreaseCount);
    </script>
</body>
</html>
```

### Steps to setup Backend server

1. Launch another EC2 instance (VM-2)
2. Install Redis server, refer "Backend-App-Readiness.md
3. Add this line "bind 0.0.0.0" to the redis.conf file inside /etc/redis/redis.conf ( This can be added any where in the file)

```
vi /etc/redis/redis.conf
```
Add "bind 0.0.0.0" --> Any where in the above file and save it

### Backend script:

```
vi /home/ubuntu/app.py
```
paste the below python in this

```python
from flask import Flask, jsonify
import redis

app = Flask(__name__)
r = redis.Redis(host='your_redis_instance_ip_here', port=6379, db=0)

@app.route('/')
def index():
    return app.send_static_file('index.html')

@app.route('/incr', methods=['POST'])
def incr():
    r.incr('counter')
    return '', 204, {'Access-Control-Allow-Origin': '*'}

@app.route('/decr', methods=['POST'])
def decr():
    r.decr('counter')
    return '', 204, {'Access-Control-Allow-Origin': '*'}

@app.route('/get', methods=['GET'])
def get():
    count = int(r.get('counter') or 0)
    return jsonify(count=count), 200, {'Access-Control-Allow-Origin': '*'}

if __name__ == '__main__':
    app.run(host='0.0.0.0', port=8080)
```

### Run the backend script.
```
python3 app.py
```

Replace `your_ec2_instance_ip_here` with the IP address of your EC2 instance that's hosting the backend server, and `your_redis_instance_ip_here` with the IP address of your EC2 instance that has redis.

### Test it from the frontend hosted on port 80 of your first EC2 instance.


### Conclusion:
You should now have a basic understanding of how to set up a Redis-backed counter using Flask and Python. This is a simple example, but it can be expanded upon to build more complex applications.
