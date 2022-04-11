# hashcat-podman

| License | Versioning |
| ------- | ---------- |
| [![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](https://opensource.org/licenses/MIT) | [![semantic-release](https://img.shields.io/badge/%20%20%F0%9F%93%A6%F0%9F%9A%80-semantic--release-e10079.svg)](https://github.com/semantic-release/semantic-release) |

Hashcat Podman.


## Cloning repositories

Clone the following repositories:
```
git clone https://github.com/extra2000/hashcat-podman.git hashcat-podman
git clone https://github.com/extra2000/hashcat.git hashcat-podman/src/hashcat
chcon -R -v -t container_file_t hashcat-podman/src/hashcat
```

Then `cd` into the project root directory:
```
cd hashcat-podman
```


## For AMD ROCm

Build image:
```
podman build -t extra2000/hashcat-rocm -f Dockerfile.rocm .
```

Build `hashcat` source:
```
podman run -it --rm --user root -v ./src/hashcat/:/opt/src:rw --workdir /opt/src localhost/extra2000/hashcat-rocm:latest make
```

Load SELinux policy:
```
cp -v selinux/hashcat_rocm_podman.cil{.example,}
sudo semodule -i selinux/hashcat_rocm_podman.cil /usr/share/udica/templates/base_container.cil
```

Run benchmark:
```
podman run -it --rm --user root -v ./src/hashcat/:/opt/src:rw --workdir /opt/src --device=/dev/kfd --device=/dev/dri --security-opt label=type:hashcat_rocm_podman.process localhost/extra2000/hashcat-rocm:latest ./hashcat -b --benchmark-all | tee report.txt
```
