# xboard-distro
xboard with oauth2

#### 本仓库为xboard oauth2实现。Releases包为包含xboard-distro docker镜像。

<br>

## xboard-oauth2介绍

该xboard oauth2的实现,为本人oldfriendme 注册页面的oauth2实现来移植到xboard 的项目，与本人网站的ouath2实现 大部分代码相同。

该xboard oauth2 的UrlPath入口地址默认也为为 **/oauth_auto_login**

以下为一些默认oauth2地址

回调地址：/oauth2/reg

注册地址：/oauth2/login

<br><br>

## xboard-distro介绍

#### xboard-distro是什么
xboard-distro是一个集合了前端服务（xboard）与后端服务（xrayr，hy2），反代web服务（caddy），以及oauth2服务（xboard oauth2）的集成Releases版本，用户不用在手动配置前后端，反代，即可一键使用docker启动。

<br>

#### xboard-distro简单测试：

#### 1.首先你已安装docker
```bash
docker ps
```
正常显示

<br>

#### 2.下载xboard-distro镜像
```bash
wget https://github.com/oldfriendme/xboard-distro/releases/download/v1/xboard-distro.tar.gz
```

<br>

#### 3.导入镜像到docker
```bash
docker import xboard-distro.tar.gz xboard-distro:v1
```

<br>

#### 3.简单运行测试
```bash
#为了在非特权模式下监听http/https
echo 79 > /proc/sys/net/ipv4/ip_unprivileged_port_start
docker run -d --name xb1 --network host  xboard-distro:v1 /sbin/init
# 或者使用-p 手动映射端口。
```
注：这里为了简单，直接使用宿主机网络

<br>

#### 4.初始化xboard-distro容器里面的服务
```bash
#进入容器内部
docker exec -it xb1 /bin/ash
#初始化安装服务
php service init
```

**这里会显示用户名,密码，后台管理路径，以及其他路径，只显示一次，记得保存下来**

<br>

#### 4.启动xboard-distro容器里面的服务
```bash
#在容器内部，启动服务
php service start
```
**启动大概会花上几秒**，如果显示**finished**了，那么就启动完成了，可以ctrl+C退出容器

<br>

#### 5.浏览器测试
web面板默认监听端口为0.0.0.0:16443，如果使用cdn，记得使用iptables设置cdn的白名单。

<br>

#### 6.登录后台修改配置
登录127.0.0.1:16443（你的面板地址）127.0.0.1:16443/{adminpath}，修改你的设置

<br>

**以下是一些默认启动的服务：**

0.0.0.0:16443 **（xboard面板）**

0.0.0.0:16443/oauth_auto_login **（xboard-oauth2服务）**

ws-0.0.0.0:16443/{wspath} **（caddy反代）**

grpc-0.0.0.0:16443/{grpcpath} **（caddy反代）**

REALITY  0.0.0.0:443 **（默认未配置key，需要配置才能使用）**

hy2 0.0.0.0 4443（udp） **(可以修改为其他端口)**

### 许可证

xboard-distro与xboard-oauth2以MIT许可证发布，其内部集成组件（xrayr,caddy,xboard,hy2）以原版许可证发布，参考原许可证

<br>

#### 注意，本项目以MIT许可证发布，请遵循MIT许可证的要求，特别是最后一条：
```md
THE SOFTWARE IS PROVIDED "AS IS", WITHOUT WARRANTY OF ANY KIND, EXPRESS OR
IMPLIED, INCLUDING BUT NOT LIMITED TO THE WARRANTIES OF MERCHANTABILITY,
FITNESS FOR A PARTICULAR PURPOSE AND NONINFRINGEMENT. IN NO EVENT SHALL THE
AUTHORS OR COPYRIGHT HOLDERS BE LIABLE FOR ANY CLAIM, DAMAGES OR OTHER
LIABILITY, WHETHER IN AN ACTION OF CONTRACT, TORT OR OTHERWISE, ARISING FROM,
OUT OF OR IN CONNECTION WITH THE SOFTWARE OR THE USE OR OTHER DEALINGS IN THE
SOFTWARE.
···

