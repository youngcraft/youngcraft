# Scapy Fuzz 模块解析

很久以来，scapy都是一个交互式发包的神器，因为定制所以强大，借助scapy可以方便实现很多功能，但是对于其很多功能，并没有人给予很多深入的解释，因此在此苟且用自己粗糙的语言尝试解析一遍scapy源码。


# Fuzz模块

scapy在用户手册中，提到一个用户模块，就是fuzz功能

```
>>>send(IP(dst="target")/fuzz(UDP()/NTP(version=4)),loop=1
................
Sent 16 packets.
```
然后我们来解析下发送的数据包
![](http://)


# 源码解读 Fuzz模块

## field 字段对fuzz的支持

```
I:\code\scapy\scapy\fields.py:
 1081  
 1082  class ByteEnumKeysField(ByteEnumField):
 1083:     """ByteEnumField that picks valid values when fuzzed. """
 1084      def randval(self):
 1085          return RandEnumKeys(self.i2s)
 ....
 1087  
 1088  class ShortEnumKeysField(ShortEnumField):
 1089:     """ShortEnumField that picks valid values when fuzzed. """
 1090      def randval(self):
 1091          return RandEnumKeys(self.i2s)
 ....
 1093  
 1094  class IntEnumKeysField(IntEnumField):
 1095:     """IntEnumField that picks valid values when fuzzed. """
 1096      def randval(self):
 1097          return RandEnumKeys(self.i2s)
```

可以看到每个域，都对fuzz中字段变化方式做了一定探索


## fuzz field 递归生成packet包

通过递归产生包结构，对响应产生的相应包结构，包结构发现必须
```




```




