# CollAFL

***This is a implimentation of [CollAFL: Path Sensitive Fuzzing](https://ieeexplore.ieee.org/abstract/document/8418631).***

***[AFL 2.57b](https://github.com/google/AFL)***

***Ubuntu 22.04.1 LTS***

***[llvm 11.1.0](https://llvm.org/)***

***clang 11.1.0***


 ***Referencing and considering [other implimentations](https://github.com/dwfault/CollAFLplusplus), the instrumentation code by llvm is focused on, while the three seed selection policies are implemented merely simply in this version of CollAFL.***

***When using afl-clang-fast/afl-clang-fast++ to compile files, the llvm pass writes the information, e.g. the number of memory access within a basic block, to /tmp in order  to deliver instrumentation information to afl-fuzz, so It’s worth noticing that such files may be cleared by system, i.e. , when reboot, thus you could move generated files to the path of the binary program that will be tested.*** 

***Another thing you should know is that you must keep the -o argument same as the compiled filename without extension name when using afl-clang-fast/afl-clang-fast++. Because llvm pass decides the filename that will be wrote to /tmp according to Module.getName() when using afl-clang-fast/afl-clang-fast++. If the filename is not consistent, CollAFL won’t find the right path.***

***last but not least, it’s fine to continue fuzzing if CollAFL can’t find files made from llvm pass, cause the three seed selection policies are implemented so simply that it's acceptable to ignore them.***

***Moreover, when testing base64 of LAVA-M for three day, CollAFL with three seed selection policies finds 217 paths while AFL finds 151 paths. However, CollAFL with three seed selection policies executes three times slower than AFL.*** :laughing:***c'est la vie.*** 

### Install

If using [docker](https://www.docker.com/), run the following command to automatically build the docker image.

```sh
git clone https://github.com/Eterniter/CollAFL-2.57b
cd CollAFL-2.57b
# build docker image
docker build -t collafl --no-cache ./
```
If not, compile the project just like AFL.One example is shown below.
```sh
cp /etc/apt/sources.list /etc/apt/sources.list.bak 
sed -i "s:/archive.ubuntu.com:/mirrors.tuna.tsinghua.edu.cn/ubuntu:g" /etc/apt/sources.list 
apt-get clean 
apt-get update --fix-missing 
apt-get install -y wget sudo build-essential libacl1-dev git --fix-missing 

apt-get install -y clang-11 llvm-11 --fix-missing 
ln -s /usr/bin/clang-11 /usr/bin/clang 
ln -s /usr/bin/clang++-11 /usr/bin/clang++ 
ln -s /usr/bin/llvm-ar-11 /usr/bin/llvm-ar 
ln -s /usr/bin/llvm-as-11 /usr/bin/llvm-as 
ln -s /usr/bin/llvm-config-11 /usr/bin/llvm-config 

git clone https://github.com/Eterniter/CollAFL-2.57b
cd CollAFL-2.57b
make 
cd llvm_mode 
make 
cd ..
make install 
```


### Example

Run the following command to test the program called 'base64' of [LAVA-M](https://ieeexplore.ieee.org/document/7546498).


```sh
cd CollAFL-2.57b/test
sed -i 's/\r$//' ./fuzz_LAMA-M_base64.sh
sh ./fuzz_LAMA-M_base64.sh
```

