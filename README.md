Setup

    vagrant up
    vagrant ssh leader
    docker swarm init --advertise-addr 192.168.50.2
    exit
    vagrant ssh follower
    docker swarm join --token <token-from-leader-swarm-init> 192.168.50.2:2377
    exit
    vagrant ssh leader
    cd /vagrant
    docker stack deploy -c docker-compose.yml hello-world

On host access one or the other:

    http://192.168.50.2:4000/
    http://192.168.50.3:4000/

Stack visualizer

    http://192.168.50.2:4100/
    http://192.168.50.3:4100/

Things to try. Suspend the "follower" vm and see what happens:

    vagrant suspend follower


Open in a browser:
    
    http://192.168.50.2:4100/