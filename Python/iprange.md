# IP 范围类
项目地址: [IP范围python项目](https://github.com/SupperDragon/iprange)  
该类用于构造一个IP范围对象, 并测试某个IP是否包含于这个IP范围,同时支持IPv4与IPv6地址. 

## 初始化IP范围对象
支持多种格式初始化一个IP范围对象
```python
import iprange
myrange = iprange.IpRange()

# 支持主机地址字符串初始化
myrange.set_range("192.168.1.2")
myrange.set_range("2001:db8::1000")

# 支持32/128 bit位整形IP初始化
myrange.set_range(3232235777) # 192.168.1.1
myrange.set_range(42544930009679042476373200873331884272) # 2001:db00::f0

# 支持packed数据初始化
ipv4_packed = b'\xC0\xA8\x00\x01'
myrange.set_range(ipv4_packed) # 192.168.0.1
ipv6_packed = b' \x01\r\xb8\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x10\x00'
myrange.set_range(ipv6_packed) # '2001:db8::1000'

# 用前缀式网络地址初始化
myrange.set_range("192.168.0.0/24")
nself.myrange.set_range("2001:db00::/112")

# 支持用子网掩码式网络地址初始化(仅IPv4)
netv4_str = "192.168.0.0/255.255.255.0"
myrange.set_range(netv4_str)

# 支持用IP区间初始化
ipv4_sec = "192.168.0.0-192.168.0.255"
myrange.set_range(ipv4_sec)
ipv6_sec = "2001:db00::000f-2001:db00::00ff"
myrange.set_range(ipv6_sec)

```

## 测试一个IP地址是否包含在某个IP范围内
IpRange.contain(ip)方法用于测试一个IP地址是否包含在某个IP范围内.
```python
import iprange

ipv4range = IpRange()
ipv4range.set_range("192.168.123.0/24")
ipv6range = IpRange()
ipv6range.set_range("2001:db00::f-2001:db00::ff")

# 测试一个IPv4地址是否包含于某个IPv6范围内,或者
# 测试一个IPv6地址是否包含于某个IPv4范围内,都返回False
ipv6range.contain("192.168.1.1") # False
ipv4range.contain("2001:db00::e") # False

# 被测试的IP包含于IP范围内时, 返回True
ipv4range.contain("192.168.123.0") # True
ipv4range.contain("192.168.123.255") # True
ipv4range.contain(3232267136) # 192.168.123.128, True
ipv6range.contain("2001:db00::f") # True
ipv6range.contain(42544930009679042476373200873331884272) # 2001:db00::f0, True
ipv6range.contain("2001:db00::ff") # True

# 被测试 的IP超出国IP范围时, 返回False
ipv4range.contain("192.168.122.255") # False
ipv4range.contain("192.168.124.0") # False
ipv6range.contain("2001:db00::e") # False
ipv6range.contain("2001:db00::100") # False
```
