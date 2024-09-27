```bash
# User specific aliases and functions
# where need proxy
proxy() {
    export PROXY_IP=192.168.16.186
    export PROXY_PORT=18899
    export NO_PROXY=127.0.0.1,10.0.0.0/8,172.16.0.0/12,192.168.0.0/16,localhost,.local,.liangkui.co
    export https_proxy=http://${PROXY_IP}:${PROXY_PORT} http_proxy=http://${PROXY_IP}:${PROXY_PORT} all_proxy=socks5://${PROXY_IP}:${PROXY_PORT}
    export HTTPS_PROXY="${https_proxy}" HTTP_PROXY="${http_proxy}" ALL_PROXY="${all_proxy}"
    echo "System http, https, socks5 Proxy on, proxy ip: ${PROXY_IP}, proxy port: ${PROXY_PORT}"
}

# where need noproxy
noproxy() {
    unset https_proxy http_proxy all_proxy HTTPS_PROXY HTTP_PROXY ALL_PROXY
    echo "System http, https, socks5 Proxy off"
}
```

