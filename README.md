# setup

## Modern Robotics Platform 
The Modern Robotics Platform's goal is to bring robotics more in line with modern  
software development. This means using modern tools and practices in the creation  
of robotics applications. This means using tools that are familiar to developers,  
such as:  
- Raspberry Pi 4
- Redis for PubSub and Key Value storage  
- Docker for running services  
- Kubernetes for container orchestration  
- HTTP, web sockets for communication
- YAML for configuration


### Enable cgroup_memory
`/boot/firmware/cmdline.txt`
```
cgroup_enable=memory swapaccount=1 cgroup_memory=1 cgroup_enable=cpuset
```

### Create UDEV rule for i2c
`/etc/udev/rules.d/99-i2c.rules`
```
KERNEL=="i2c-[0-7]",MODE="0666"
```

### Need to blacklist 
`/etc/modprobe.d/blacklist-industialio.conf`
```
blacklist st_magn_spi
blacklist st_pressure_spi
blacklist st_sensors_spi
blacklist st_pressure_i2c
blacklist st_magn_i2c
blacklist st_pressure
blacklist st_magn
blacklist st_sensors_i2c
blacklist st_sensors
blacklist industrialio_triggered_buffer
blacklist industrialio
```

### Add modules 
`/etc/modules`  
```
i2c-dev  
i2c-bcm2708  
```

### Install microkube
```
$ sudo snap install microk8s --classic --channel=1.20/stable
$ microk8s.status
$ microk8s enable dns
```

### Get the kubectl config
```
$ microk8s config
```

### Install the helm charts 
```
# Inginx ingress
$ helm repo add ingress-nginx https://kubernetes.github.io/ingress-nginx
$ helm install ingress ingress-nginx/ingress-nginx

# Modern Robotics Platform charts
$ helm repo add collective https://chartmuseum.the-collective-group.com
$ helm repo update

$ helm upgrade -i redis collective/redis
$ helm upgrade -i redis-proxy collective/redis-proxy
$ helm upgrade -i web-app collective/web-app
$ helm upgrade -i sense-hat collective/sense-hat
```
