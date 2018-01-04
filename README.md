## Steps

Attempting to attach webstorm's debugger to an express app, which uses Node's cluster, running via docker compose

1. git clone https://github.com/mendhak/webstorm-dockercompose-cluster-debug.git
2. Open that project in WebStorm, run `npm install`
3. Ensure there's a breakpoint in routes/index.js [on line 6](https://github.com/mendhak/webstorm-dockercompose-cluster-debug/blob/master/routes/index.js#L6).
4. Look at the bin/www file and notice how the cluster.fork() is being created. One [child for each CPU core](https://github.com/mendhak/webstorm-dockercompose-cluster-debug/blob/master/bin/www#L22-L24).
5. In terminal, run `sudo docker-compose up` - you should see the container come up, and start listening.
6. Then from the debug configurations choose to run 'dc-attach' - it attaches to port 5858

## Problem

Now - browse to http://localhost:3000 and the breakpoint does not get hit. This is probably because the actual work is now done in the children, listening on ports 5859, 5860, 5861, 5862.

## Question 

How to get WebStorm's debugger to recognize this forking and attach itself to the right places? 
