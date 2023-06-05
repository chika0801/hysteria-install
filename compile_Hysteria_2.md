```
curl -sLo go.tar.gz https://go.dev/dl/go1.20.4.linux-amd64.tar.gz
tar -C /usr/local -xzf go.tar.gz
rm go.tar.gz
echo -e 'export PATH=$PATH:/usr/local/go/bin' > /etc/profile.d/go.sh
source /etc/profile.d/go.sh
go version
```

```
apt install -y git python3
```

```
git clone -b wip-hy2 --single-branch https://github.com/apernet/hysteria.git hy2
cd hy2
export HY_APP_PLATFORMS="windows/amd64,linux/amd64"
echo $HY_APP_PLATFORMS
python3 ./hyperbole.py build
```
