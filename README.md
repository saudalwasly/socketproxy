# README #
`evhost` is a simple yet efficient Python-based gateway server that directs incoming traffic to different destinations based on rules stated in the configuration file. `evhost` is based on `gevent` which allows `evhost` to handle many requests per second, e.g 10K requests per second. I made `evhost` because I wanted to host several services (servers) on the same machine, but I did not want to install full-blown server such as Appache server. In addition, I wanted `evhost` to be lightweight and fast. `evhost` is contained in one python file only, which makes it easy to deploy.

### How to install `evhost` ###
Download `evhost` and copy it to the desired destination directory, that is it!. 
If you want to install it in a system-wide settings, you can copy it to `/usr/local/bin/`

Currently, `evhost` depends on `gevent` Python package. You can install `gevent` using `pip` as following:
```
sudo pip install gevent
```

`authbind` is an optional good addition. `authbind` allows binding to privilege ports without being root. Here is a good introduction about `authbind` [(The link)](https://mutelight.org/authbind).
### Using evhost ###
```
#> evhost -h
#> ./evhosts -h
Usage:
    evhosts <local address> <port> <settings file>
Example:
    evhosts localhost 8080 settings.yaml
#> 

```

### Configuration File ###
The configuration file is a text-based yaml-like file. An example of a configuration file is listed below. `evhost` detects changes in the configuration file at runtime and adopts the changes right away.

```yaml
<Host>:
  - localhost
  - your.domain.something
  - 127.0.0.1
</Host>

<Defualt Target>:
  - Target Host: localhost
  - Target Port: 8000
  - Root: /
</Defualt Target>


<Sub Domain>:
  - Sub Domain: site1
  - Target Host: localhost
  - Target Port: 1500
  - Root: /
</Sub Domain>

<Sub Domain>:
  - Sub Domain: site2.x
  - Target Host: localhost
  - Target Port: 1502
  - Root: /
</Sub Domain>

<Sub Domain>:
  - Sub Domain: g
  - Target Host: www.google.com
  - Target Port: 80
  - Root: /news
</Sub Domain>

<Redirect>:
  - Source URL: r.localhost/url1
  - Target URL: http://www.uwaterloo.ca
  - Redirection Type: 303
</Redirect>

<Redirect>:
  - Source URL: r.localhost/url2
  - Target URL: http://www.google.ca
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