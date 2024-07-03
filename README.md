更新说明：
==============
之前compile文件打包路径为相对路径，非绝对路径，导致每次都需要进入nginx路径内去启动，如果我们需要考虑用服务控制，去编写nginx.service时，需要先进入nginx路径后，执行/sbin/nginx启动，这无疑增加了毫无营养的工作量。

最终解决办法：
将nginx路径固定到/app/nginx路径，将配置文件conf固定到/app/nginx/conf/nginx.conf，这样就可以使用service来控制nginx

同时，考虑到每次编译nginx后，都需要手动修改nginx.service，我也一并将nginx.service进行打包到nginx根路径下，且编写了一个start.sh脚本，用于服务注册。

简而言之：
这个版本Nginx非常完善，编译路径：/app/nginx 附带了nginx.service，后续nginx升级后，只需要正常编译，将nginx文件替换到/app/nginx/sbin/nginx即可。

有个地方需要注意：
部分centos7设备无法正常编译，会提示：
make: *** No rule to make target `build’, needed by `default’. Stop.
建议换机器编译，原因是nginx自己内部的问题，暂无找到其他解决办法。

同时，麒麟v10操作系统，可以正常使用Debian11进行编译后的包。

nginx-portable
==============

nginx-portable is a portable version of the nginx web server for linux.  
At this point in time the package contains nginx-1.10.2 or nginx-1.11.8 with the --latest flag; so no fastcgi and mysql just yet.

#### About nginx

> nginx [engine x] is an HTTP and reverse proxy server, as well as a mail proxy server, written by Igor Sysoev  

You can read all about it on the [nginx website](http://nginx.org/en/)  

## Goals
What is nginx-portable all about?
* A standalone, pre-configured, portable, stable version of the nginx web server for linux
* Easily distributable package together with or as base for other products

## Dependencies
On Ubuntu installing the following packages should provide all the files necessary to build nginx
```
sudo apt-get install curl build-essential libpcre3-dev libssl-dev
```
On CentOS installing the following packages should provide all the files necessary to build nginx
```
yum install gcc-c++
yum install pcre pcre-devel
yum install zlib zlib-devel
yum install openssl openssl-devel
```

## Getting started
1. Clone this repository
2. To compile a binary for your architecture issue `./compile`.
3. Run `sudo ./nginx-portable start` to start the server
4. Fire up your favorite browser and go to `http://localhost:8080`
Voila!

Congratulations, your nginx server is up and running.  
Now go on and add your own files to the `html` directory.

## Usage

#### The init script
The init script works pretty much exactly like nginx init.d script on Ubuntu.
```
Usage: ./nginx-portable {start|stop|restart|reload|test}
```

#### Configuration
If you need to change any of the default values you can find `nginx.conf` under
`conf/nginx.conf` in the nginx-portable directory.

### Customization
Additional modules and flags can be added to/removed from the appropriate line in the `compile` file.

## Known issues
* This package has only been tested on Ubuntu Server 12.04 LTS 32bit, Ubuntu Server 13.10 64bit and Ubuntu Server 14.04 64bit.
* The compile script currently does not work correctly under Mac OSX. You can get it to run by hardcoding the scripts absolute path to `BASEDIR` in the compile script.

If your find any bugs or have suggestions on how to improve nginx-portable, feel free to write up an issue here on GitHub or fork the repo to tinker with it yourself.

## Boring legal stuff

THE SOFTWARE IS PROVIDED "AS IS" AND THE AUTHOR DISCLAIMS ALL WARRANTIES WITH REGARD TO THIS SOFTWARE INCLUDING ALL IMPLIED WARRANTIES OF MERCHANTABILITY AND FITNESS. IN NO EVENT SHALL THE AUTHOR BE LIABLE FOR ANY SPECIAL, DIRECT, INDIRECT, OR CONSEQUENTIAL DAMAGES OR ANY DAMAGES WHATSOEVER RESULTING FROM LOSS OF USE, DATA OR PROFITS, WHETHER IN AN ACTION OF CONTRACT, NEGLIGENCE OR OTHER TORTIOUS ACTION, ARISING OUT OF OR IN CONNECTION WITH THE USE OR PERFORMANCE OF THIS SOFTWARE.
