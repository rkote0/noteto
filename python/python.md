# Python

## 日志打印

```python
encond=404
msg='xxx'
print(f"{encond=} {msg=}")

# > encond=404 msg='xxx'
```





## 支持变长解包

```PYTHON
if (n := len(msg)) > 1:
    print(f"{encond=} {msg=}")
else:
    print(f"{encond=} error")


s = {'xiaoming': [180, 50, '江苏']}
s1 = {'xiaoming': {
    'height': 180,
    'weight': 50,
    'city': '江苏'
}}

s2 = {'xiaoming': [180, 50, '江苏', 'man']}
s3 = {'xiaoming': [180, 50, '江苏', 'man', 'CHINA']}

for k,(V1, V2, V3) in s.items():
    print(k, V1, V2, V3)

for key, value in s.items():
    print(key, value[0])


for key, (v1,v2, *rest, v3) in s3.items():
    print(key, v1, v2, v3, rest, sep=',')
    print(rest[0])


for key, value in s1.items():
    print(key,value)

for key,(V1, V2, V3) in s.items():
    print(key, V1, V2, V3,sep='-')
    
    
# demo
a, *rest, b = range(4)
print(a, b, rest)
# > 0 3 [1, 2]

*rest, c = range(4)
print(rest, c)
# > [0, 1, 2] 3
```



