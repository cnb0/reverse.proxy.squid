Setup a reverse proxy to provide temporary internet connection to an EC2 instances running in a private zone. The assumption is that your request goes via a jump box. Connection to internet might be required during development phase or to update patches.

The idea is that a remote machine connects to the squid proxy running on your local machine via ssh reverse tunnel. In the example below fio was installed on a remote machine for benchmarking disk I/O.

This reverse proxy can also be used for installing packages such as pgadmin or jupyter-hub during development phase

#### Open a terminal on your local machine and start squid container
`docker-compose up`

#### open a new terminal and connect to reverse proxy on localhost. The [HOST] here is the remote machine IP
```
ssh -R 19999:localhost:3128 [HOST]
sudo su -
vi /etc/apt/apt.conf.d/squidProxy
- Acquire::http::Proxy "http://localhost:19999";

apt-get update
install packages:
- apt-get install fio
- fio --help

export variables if you would like to use things like wget:
- export http_proxy=http://localhost:19999
- export https_proxy=http://localhost:19999
- export ftp_proxy=http://localhost:3128
```

### Reference:
- [sameersbn/docker-squid](https://github.com/sameersbn/docker-squid)
