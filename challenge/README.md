STI Technical Challenge
=======================

## The Challenge

Your challenge, should you choose to accept it, is to set up and
maintain a sustainable web application that can be easily deployed and maintained, using either _Ansible_ or _Docker_ (see below),

We like people who are able to learn and research. Even if you might
not get the tasks completed, write down the notes with desired
solution. Your notes will help us in the evaluation of your
work. We understand there's more than one way to do things. 

---
## Specifications

The deployment setup should be created and managed by either_Ansible_ or _Docker_. 


### Ansible

It should contain at least 3 nodes:

- A balancer node with _Haproxy_ on board.
- An application node with a _Django_ or _Node.js_ application (should be served
  with nginx).
- A backend db node with _Postgres_ or _MySQL_.

### Docker

It should contain at least 3 containers:

- A load balancer container using traefik.
- An application container with a _Django_ or _Node.js_ application.
- A backend db container with either _Postgres_ or _MySQL_.

---
## Rules

### Ansible

- Ansible should build and run all infrastructure by one play.
- Ansible should install critical security patches for the OS.
- Ansible should update application code without any interruptions of
  service for end user.
- The Nodejs or Django application does not need to be complex or written
  specifically for this challenge. Feel free to take any ready-to-use
  open source application.
- Please try and complete the assessment within one week of receiving it.
- Explain your solutions in 2-3 lines.
- Use this repository to track and share your work. Please link
  commits relevant to each task completed.
- Share your results even if you don't finish.

### Docker

- Use a `docker-compose` file to bring the whole infrastructure at once.
- You should provide a documentation on how to update the applications containers without an interruption of service.
- The Nodejs or Django application does not need to be complex or written
  specifically for this challenge. Feel free to take any ready-to-use
  open source application.
- Please try and complete the assessment within one week of receiving it.
- Explain your solutions in 2-3 lines.
- Use this repository to track and share your work. Please link
  commits relevant to each task completed.
- Share your results even if you don't finish.

---

## Setup

### Ansible

- Install _Virtualbox_ and _Vagrant_ on your machine.
- Ensure to have a RSA key stored under "~/.ssh/id_rsa.pub".
- Copy the `Vagrantfile` on this directory to the `solution` one and then brings the machine online using `vagrant up`.

After running this Vagrantfile you will have a Class-C private network
on the 192.168.50.0 subnet with the following FQDNs and IP address for each node:

```

- node-1.vagrant : 192.168.50.10
- node-2.vagrant : 192.168.50.11
- node-3.vagrant : 192.168.50.12

```

### Docker

- Make sure _docker_ and _docker-compose_ is installed in your machine.
- The vagrant file won't be needed in this case.

---
## Tips

- Feel free to add more features! Really, we're curious about what you
  can think of. We'd expect the same if you worked with us.
- Comment your solution, either with comments on your configs or in a
  separate documents' structure.
- Use all git tools to let us understand how you arrived to the
  solution. Preferably, submit your solution with multiple consequent
  commits, and not just one.
- Be ready to answer any kind of questions about your solution.


## Bonus

- Increase failover by adding one or more application(s)/backend
  node(s) (create your own updated Vagrantfile to be able to build
  your infrastructure).
- Create _Ansible_ playbook for creating/restoring database backups.