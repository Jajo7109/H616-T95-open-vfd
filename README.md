# linux_openvfd

This repository contains the source code for the FD628 and similar LED display controllers,
[Controller datasheet](http://pdf1.alldatasheet.com/datasheet-pdf/view/232882/PTC/PT6964.html).

The FD628 controller is often used in Android TV boxes with a 7 segment LED display at the front.

Although the FD628 controller has support for a multitude of different display configurations,
the current implementation only supports common cathode and common anode displays with 7 segments.
The driver can also support FD650, which uses a variation of the I2C communication protocol.
In addition it supports HD44780 text based LCD displays, using an I2C backpack.

Courtesy of Arthur Lieberman, a version was created that allows the insertion of pin numbers greater than 255.

How to enable OpenVFD Service for TV BOX running armbian...

1. Download kernel headers for Allwinner armbian-config -> Software -> Headers_install
2. Install kernel headers with apt-get install -y build-essential linux-headers-edge-sunxi64
3. Copy openvfd.dts to /boot/dtb/allwinner/overlay/ and run armbian-add-overlay /boot/dtb/allwinner/overlay/openvfd.dts
4. reboot to install the overlays openvfd.dtb
5. cd / and git clone https://github.com/Jajo7109/H616-T95-open-vfd.git
6. Run sudo ln -sf /boot/System.map-$(uname -r) /lib/modules/$(uname -r)/build/System.map
7. Run make -j 4 within H616-T95-open-vfd/driver folder
8. Run sudo depmod -a
9. Run sudo make modules_install
10. Verify if our openvfd module is successfully installed/loaded by lsmod. If not, run sudo modprobe openvfd
11. Run make OpenVFDService within H616-T95-open-vfd folder. Add executable priviledge by running chmod +x
12. Copy cp openvfd.service /etc/systemd/system/openvfd.service
13. Run systemctl enable openvfd.service
14. Run systemctl start openvfd.service
The display should show the current time depending on the time zone.
