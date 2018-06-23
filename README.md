# Ubiquiti Networks Unifi Controller

## Usage

### Prerequisites 

```
$ sudo dnf install podman
$ sudo adduser -r -s /sbin/nologin unifi 
$ sudo mkdir -p /opt/unifi/{data,logs,run}
$ sudo chown -R unifi. /opt/unifi
$ sudo chcon -Rt svirt_sandbox_file_t /opt/unifi/
```

### Build From GitHub

sudo podman build --build-arg UNIFI_VERSION=5.8.23-d5a5bbfda4 \
    --build-arg UNIFI_UID=$(id -u unifi) \
    -t unifi:5.8.23-d5a5bbfda4 git://github.com/jdoss/unifi

### Build Locally

```
$ git clone https://github.com/jdoss/unifi
$ cd unifi
$ sudo podman build --build-arg UNIFI_VERSION=5.8.23-d5a5bbfda4 \
    --build-arg UNIFI_UID=$(id -u unifi) \
    -t unifi:5.8.23-d5a5bbfda4 .
```
### Run the Ubiquiti Networks Unifi Controller

```
sudo podman run --rm --cap-drop ALL -e UNIFI_UID=$(id -u unifi) \
  -e UNIFI_VERSION=5.8.23-d5a5bbfda4 \
  -e JVM_MAX_HEAP_SIZE=1024m \
  -e TZ='America/Chicago' \
  -p 8080:8080 -p 8443:8443 -p 8843:8843 -p 10001:10001/udp \
  -v /opt/unifi/data:/opt/unifi/data:Z \
  -v /opt/unifi/logs:/opt/unifi/logs:Z \
  -v /opt/unifi/run:/opt/unifi/run:Z \
  --name unifi localhost/unifi:5.8.23-d5a5bbfda4
```

## License

The MIT License

Copyright (c) 2018 Joe Doss

Permission is hereby granted, free of charge, to any person obtaining a copy
of this software and associated documentation files (the "Software"), to deal
in the Software without restriction, including without limitation the rights
to use, copy, modify, merge, publish, distribute, sublicense, and/or sell
copies of the Software, and to permit persons to whom the Software is
furnished to do so, subject to the following conditions:

The above copyright notice and this permission notice shall be included in
all copies or substantial portions of the Software.

THE SOFTWARE IS PROVIDED "AS IS", WITHOUT WARRANTY OF ANY KIND, EXPRESS OR
IMPLIED, INCLUDING BUT NOT LIMITED TO THE WARRANTIES OF MERCHANTABILITY,
FITNESS FOR A PARTICULAR PURPOSE AND NONINFRINGEMENT. IN NO EVENT SHALL THE
AUTHORS OR COPYRIGHT HOLDERS BE LIABLE FOR ANY CLAIM, DAMAGES OR OTHER
LIABILITY, WHETHER IN AN ACTION OF CONTRACT, TORT OR OTHERWISE, ARISING FROM,
OUT OF OR IN CONNECTION WITH THE SOFTWARE OR THE USE OR OTHER DEALINGS IN
THE SOFTWARE.