# Paddle Lite adapts to Zephyr

The Paddle Lite 2.6 version was selected for this project because this version does not contain any third-party library files.


## Running Paddle Lite on qemu_cortex_a53:

The library file is placed under zephyrproject/modules/lib.executing the following command：

west build -b  qemu_cortex_a53 samples/modules/paddlelite/

cd ~/zephyrproject/zephyr/build && ~/zephyr-sdk-0.16.1/sysroots/x86_64-pokysdk-linux/usr/bin/qemu-system-aarch64 -cpu cortex-a53 -nographic -machine virt,secure=on,gic-version=3 -m 4G -net none -pidfile qemu.pid -chardev stdio,id=con,mux=on -serial chardev:con -mon chardev=con,mode=readline -icount shift=4,align=off,sleep=on -rtc clock=vm -device loader,file=/path-to/zephyrproject/zephyr/samples/modules/paddlelite/model/mobilenet_v1.nb,addr=0x70000000,force-raw=on -kernel ~/zephyrproject/zephyr/build/zephyr/zephyr.elf

Among them, zephyr-sdk-0.16.1 selects the sdk version installed by yourself, file=/path-to/zephyrproject/zephyr/samples/modules/paddlelite/model/mobilenet_v1.nb, path-to represents your own directory, and mobilenet_v1.nb represents the selected model.

## Running Paddle Lite on roc_rk3568_pc:

west build -b  roc_rk3568_pc samples/modules/paddlelite/

setenv serverip 192.168.0.102

setenv ipaddr 192.168.0.103

tftp 0x40000000 zephyr.bin;

tftp 0x70000000 resnet18.nb;

dcache flush; icache flush; dcache off; icache off; go 0x40000000;

The above means selecting resnet18. If you want to select mobilenet_v1.nb, execute tftp 0x70000000 mobilenet_v1.nb.

## Model description:

The models are all provided by Baidu official website, and most of them are trained on the imagenet data set. 
The NB file is a binary file formed by cropping the model using the opt tool provided by Paddle Lite2.6.
