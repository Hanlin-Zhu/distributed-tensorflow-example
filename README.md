# Distributed Tensorflow Example on Google Cloud Platform (GCE)

Using data parallelism with shared model parameters while updating parameters asynchronous. See comment for some changes to make the parameter updates synchronous (not sure if the synchronous part is implemented correctly though).

Trains a simple sigmoid Neural Network on MNIST for 20 epochs on three machines using one parameter server. The goal was not to achieve high accuracy but to get to know tensorflow.

A Simple Google Cloud Platform version is detailed as the following 

Step 1. Register for Google Cloud Platform ( you get 60 days trials and $300 for free)  https://cloud.google.com/

Step 2. Go to your console (main page of GCE) and  create a compute machine instance. 
        a. click the 3 horizontal bar icon on the top left corner, it gives a lists of services and products 
        b. select compute->compute engine->create an instance
        c. use the name pc-01 and other default settings like 1vCPU, 3.75G, enable HTTP and HTTPS traffic
        
Step 3. Once pc-01 has been created. you would repeat Step2 and create pc-02, pc-03 and pc-04,A list of compute engines created should be shown together with their internal and external IPs.

Step 4. click on the SSH connect icon in the pc-01 row to access the first virtual machine. 

Step 5. Install git, python, pip, tensorflow, on pc-01 by performing the following commands:
```
   sudo apt-get update
   sudo apt-get install git 
   sudo apt-get install python-dev python-pip
   export TF_BINARY_URL=https://storage.googleapis.com/tensorflow/linux/cpu/tensorflow-0.9.0-cp27-none-linux_x86_64.whl
   sudo pip install --upgrade $TF_BINARY_URL
   
```
Step 6. clone this directory 
```
   git clone https://github.com/Hanlin-Zhu/distributed-tensorflow-example.git
```

Step 7. Repeat Step 4 and 6 for all the other three virtual machines

Step 8. Access each machine through SSH connect and run the corresponding example.py, the program won't start and may either freeze or raise connection failed error until you have run the program on all machines. 
```
pc-01$ python ./distributed-tensorflow-example/example.py --job-name="ps" --task_index=0 

pc-02$ python ./distributed-tensorflow-example/example.py --job-name="worker" --task_index=0 

pc-03$ python ./distributed-tensorflow-example/example.py --job-name="worker" --task_index=1 

pc-04$ python ./distributed-tensorflow-example/example.py --job-name="worker" --task_index=2 
```

More details here: [ischlag.github.io](http://ischlag.github.io/)
