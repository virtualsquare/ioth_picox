# libioth\_picox

## The picoxnet module for libioth

## Compile and Install

Pre-requisites: [`libioth`](https://github.com/virtualsquare/libioth), 
[`picoxnet`](https://github.com/virtualsquare/picoxnet).

Libioth uses cmake. The standard building/installing procedure is:

```bash
mkdir build
cd build
cmake ..
make
sudo make install
```

An uninstaller is provided for your convenience. In the build directory run:
```
sudo make uninstall
```

## Usage

When installed this plugin can be loaded using `ioth_newstack`.
e.g.:
```C
	struct ioth *stack = ioth_newstack("picox","vde:///tmp/mysw");
```

## testing

All the examples in the test direcotry of [`libioth`](https://github.com/virtualsquare/libioth)
can be experimented using this plugin:

```bash
     ./iothtest_server picox vde:///tmp/sw
     ./iothtest_client picox vde:///tmp/sw
```
