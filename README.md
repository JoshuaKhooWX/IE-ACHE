# Arithmetic Circuit Homomorphic Encryption - Upgrading of ECDH to SIDH
[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](https://opensource.org/licenses/MIT)
![GitHub issues](https://img.shields.io/github/issues/kennethsoh/IE-ACHE)
![GitHub repo size](https://img.shields.io/github/repo-size/kennethsoh/IE-ACHE)
![Lines of code](https://img.shields.io/tokei/lines/github/kennethsoh/IE-ACHE)

This is an extension of an ongoing research project on Arithmetic Circuit Homomorphic encryption (ACHE) which was implemented based on Fast Fully Homomorphic Encryption over the Torus (TFHE). This iteration focuses on upgrading and comparing the current authentication method using ellipctive curve diffie hellman (ECCH) to a Post Quantum authentication method, Super singular Isogeny Diffie Hellman (SIDH). The previous version of the project can be found <a href="https://github.com/kennethsoh/IE-ACHE" target="_blank">here</a>.

This project was shared at the Asia Conference on Computers and Communications (ACCC) 2021 with one paper published: 

<a href="https://ieeexplore.ieee.org/document/9681471" target="_blank">Arithmetic Circuit Homomorphic Encryption Key Pairing Comparisons and Analysis between Elliptic Curve Diffie Hellman and Supersingular Isogeny Diffie Hellman</a> 

<br>

This system is configured and tested for Ubuntu 20.04 LTS

#### Dependencies
* Python3 and Pip3
``` bash
$ sudo apt-get install python3 python3-pip

# Check python3 installation
$ which python3
> /usr/bin/python3
```

* <a href="https://pypi.org/project/asn1tools/" target="_blank">ASN.1 Tools</a>
* <a href="https://pypi.org/project/pycryptodomex/" target="_blank">PyCryptodome</a>
```bash
$ pip3 install asn1tools pycryptodomex
```

* <a href="https://github.com/tfhe/tfhe" target="_blank">Fast Fully Homomorphic Encryption over the Torus</a>
```bash
$ cd /
$ git clone --recurse-submodules --branch=master https://github.com/tfhe/tfhe.git
$ cd tfhe
$ mkdir build; cd build
$ ccmake ../src

# Press 'c' to configure, then 'g' to generate

$ make
$ make install
```

#### Installation Guide
1. Clone this repository
```bash
$ cd /
$ git clone https://github.com/JoshuaKhooWX/IE-ACHE.git
$ cd IE-ACHE

# Provide executable permissions if needed
$ chmod u+x */*
```

2. Repeat dependency installation and Step 1 minimally 4 times for 4 different machines: Client(1/2/3), Cloud, Keygen and Output.

3. Assign IP Addresses for machines as follows:<br>
```
Cloud:    192.168.0.1
Client1:  192.168.0.21
Client2:  192.168.0.22
Client3:  192.168.0.23
Keygen:   192.168.0.3
Output:   192.168.0.4
```

4. Add the service files in 'Services' folder to ```/etc/systemd/system/```. Edit the <u>Users</u>, <u>WorkingDirectory</u> and <u>ExecStart</u> values if necessary. Default values assume that this repository is installed at root (/) directory and root user is used. 

5. Recompile c files
```bash
$ python3 compile_c.py
> 9 files compiled
```

6. Start the services on each machine
```
$ systemctl daemon-reload
$ systemctl enable MP MP2
$ systemctl start MP MP2
```

7. Run ./process on all Client machines to generate values.txt
```
Output$ cd /IE-ACHE/Client{1..3}
Output$ ./process
```

8. Run output_dynamic.py on Output machine
```
Output$ cd /IE-ACHE/Output
Output$ python3 output_dynamic.py
```
