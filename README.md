# sixpack-gateway
CAN-MQTT Gateway for SixPack Home Automation

## Boards / Tranceivers:

### 2-CH_CAN_FD_HAT (Waveshare)

https://www.waveshare.com/wiki/2-CH_CAN_FD_HAT

`/boot/config.txt`
```text
dtparam=spi=on
dtoverlay=spi1-1cs,cs0_pin=26
dtoverlay=mcp251xfd,spi0-0,interrupt=25
```

Check with `dmesg | grep spi`:

```text
[    5.638547] spi_master spi0: will run message pump with realtime priority
[    5.651539] mcp251xfd spi0.0 can0: MCP2518FD rev0.0 (-RX_INT -MAB_NO_WARN +CRC_REG +CRC_RX +CRC_TX +ECC -HD c:40.00MHz m:20.00MHz r:17.00MHz e:16.66MHz) successfully initialized.
[    5.662376] spi_master spi1: will run message pump with realtime priority
[    5.681702] mcp251xfd spi1.0 can1: MCP2518FD rev0.0 (-RX_INT -MAB_NO_WARN +CRC_REG +CRC_RX +CRC_TX +ECC -HD c:40.00MHz m:20.00MHz r:17.00MHz e:16.66MHz) successfully initialized.
```

### RS485/CAN Hat (Waveshare)

https://www.waveshare.com/wiki/RS485_CAN_HAT


`/boot/config.txt`
```text
dtparam=spi=on
dtoverlay=mcp2515-can0,oscillator=12000000,interrupt=25,spimaxfrequency=2000000
```

Check with `dmesg | grep spi`:

```text
[    5.425099] mcp251x spi0.0 can0: MCP2515 successfully initialized.
```

## Automatically bringing can networks up

Bring one or more can interfaces up at boot with `systemd-networkd`

`/etc/systemd/network/80-can.network`

```text
[Match]
Name=can0 can1

[CAN]
BitRate=500K
RestartSec=100ms
```

```shell
sudo systemctl start systemd-networkd
sudo systemctl enable systemd-networkd
```

[More info](https://www.pragmaticlinux.com/2021/07/automatically-bring-up-a-socketcan-interface-on-boot/)
