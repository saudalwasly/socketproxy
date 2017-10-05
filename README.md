# README #
`evhost` is a simple yet efficient Python-based reverse proxy server works on the transport layer to directs incoming traffic to different destinations based on rules stated in a configuration file. `evhost` can be used to allow multiple web servers to live on the same machine with the same IP address.

 `evhost` is based on `gevent` which allows `evhost` to handle many requests per second, e.g 10K requests per second. I made `evhost` because I wanted to host several services (servers) on the same machine, but I did not want to install full-blown server such as Appache server. In addition, I wanted `evhost` to be lightweight and fast. `evhost` is contained in one python file only, which makes it easy to deploy. `evhost` supports both `http` and `https` traffice. However, in the case of `https`, mapping domains or subdomains to different `url` paths is not possible as the `https` requests are encrypted which contains the `url` path. `evhost` is totally transparent and forwards packets forth and back without modification. Unlike other servers that allows virtual hosts, `evhost` works on the socket layer (TCP/IP) and delegates all `SSL/TLS` negotiation to the destination servers (virtual hosts); as a result, `evhost` does not need to host any private/public keys or certificates of the virtual hosts. This setup, allows each virtual host to manage its own business in isolation from others.

### How to install `evhost` ###
Download `evhost` and copy it to the desired destination directory, that is it!. 
If you want to install it in a system-wide settings, you can copy it to `/usr/local/bin/`

Currently, `evhost` depends on `gevent` Python package. You can install `gevent` using `pip` as following:
```
sudo pip install gevent
```

### authbind ###

`authbind` is an optional good addition. `authbind` allows binding to privilege ports without being root. Here is a good introduction about `authbind` [(The link)](https://mutelight.org/authbind).

to install `authbind` in Ubuntu, use the following command:
```
sudo apt-get install authbind
```
To configure `authbind`, you need to make read-only files in `/ect/authbind/???/`. For example, to allow binding to `443` and `80` ports use the following commands:
```
sudo touch /etc/authbind/byport/443
sudo chown <user> /etc/authbind/byport/443
chmod 500 /etc/authbind/byport/443
```
```
sudo touch /etc/authbind/byport/80
sudo chown <user> /etc/authbind/byport/80
chmod 500 /etc/authbind/byport/80
```


### Using evhost ###
```
#> evhost -h
#> ./evhosts -h
Usage:
    evhosts <settings file>
Example:
    evhosts settings.conf
#> 

```
To use `evhost` with authbind:
```
authbind python evhost localhost 80 settings.yaml 
```

### Configuration File ###
The configuration file is a text-based yaml-like file. An example of a configuration file is listed below. `evhost` detects changes in the configuration file at runtime and adopts the changes right away.

```yaml
<Host>:
  - localhost
  - your.domain.something
  - 127.0.0.1
  - xxx.xxx.xxx.xxx
</Host>

<Bind>:
  - IPs: 0.0.0.0
  - Ports: 443, 80
</Bind>

<Defualt Target>:
  - Target Host: localhost
  - Target Port: 8000
  - Root: /
</Defualt Target>


<Domain>:
  - Domain Name: site1
  - Listening Ports: 443
  - Target Host: localhost
  - Target Port: 1500  
  - Root: /
</Domain>

<Domain>:
  - Domain Name: site2
  - Listening Ports: 80
  - Target Host: localhost
  - Target Port: 1502
  - Root: /
</Domain>
###########################################
###########################################

<Redirect>:
  - Source URL: r.site1/url1
  - Target URL: https://www.uwaterloo.ca
  - Redirection Type: 303
</Redirect>

<Redirect>:
  - Source URL: r.site2/url2
  - Target URL: http://site2/loging
  - Redirection Type: 301
</Redirect>
```



### License ###
`evhost` is distributed under the following license:

GNU GENERAL PUBLIC LICENSE Version 2, June 1991
The license document is included in the repository (`license_gpl-2.0.txt`)

Copyright (c) 2016 Saud Wasly

-----

The above copyright notice and this permission notice shall be included in all copies or substantial portions of the Software.

THE SOFTWARE IS PROVIDED "AS IS", WITHOUT WARRANTY OF ANY KIND, EXPRESS OR IMPLIED, INCLUDING BUT NOT LIMITED TO THE WARRANTIES OF MERCHANTABILITY, FITNESS FOR A PARTICULAR PURPOSE AND NONINFRINGEMENT. IN NO EVENT SHALL THE AUTHORS OR COPYRIGHT HOLDERS BE LIABLE FOR ANY CLAIM, DAMAGES OR OTHER LIABILITY, WHETHER IN AN ACTION OF CONTRACT, TORT OR OTHERWISE, ARISING FROM, OUT OF OR IN CONNECTION WITH THE SOFTWARE OR THE USE OR OTHER DEALINGS IN THE SOFTWARE.