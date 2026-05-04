import smbus
import time


class MCP3021:
    def __init__(self, mx, vb=False):
        self.bus = smbus.SMBus(1)
        self.mx = mx
        self.addr = 0x4D
        self.vb = vb

    def deinit(self):
        self.bus.close()

    def get_number(self):
        d = self.bus.read_word_data(self.addr, 0)

        lo = d >> 8
        up = d & 0xFF

        n = (up << 6) | (lo >> 2)

        if self.vb:
            print(f"Данные: {d}, upper: {up:x}, lower: {lo:x}, число: {n}")

        return n

    def get_voltage(self):
        n = self.get_number()
        v = n / 1023 * self.mx
        return v


if __name__ == "__main__":
    a = MCP3021(5.11, False)

    try:
        while True:
            v = a.get_voltage()
            print(f"Напряжение: {v:.3f} В")
            time.sleep(1)

    finally:
        a.deinit()
raspi-gpio set 2 a0
raspi-gpio set 3 a0
raspi-gpio get
