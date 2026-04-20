import smbus


class MCP4725:
    def __init__(self, vmax, addr=0x61, verbose=True):
        self.bus = smbus.SMBus(1)

        self.addr = addr
        self.wm = 0x00
        self.pds = 0x00

        self.verbose = verbose
        self.vmax = vmax

    def deinit(self):
        self.bus.close()

    def set_number(self, n):
        if not isinstance(n, int):
            print("На вход ЦАП можно подавать только целые числа")
            return

        if not (0 <= n <= 4095):
            print("Число выходит за разрядность MCP4725 (12 бит)")
            return

        b1 = self.wm | self.pds | (n >> 8)
        b2 = n & 0xFF

        self.bus.write_byte_data(self.addr, b1, b2)

        if self.verbose:
            print(f"Число: {n}, отправленные по I2C данные: [0x{(self.addr << 1):02X}, 0x{b1:02X}, 0x{b2:02X}]\n")

    def set_voltage(self, v):
        if not (0.0 <= v <= self.vmax):
            print(f"Напряжение выходит за динамический диапазон ЦАП (0.00 - {self.vmax:.2f} В)")
            return

        n = int(v / self.vmax * 4095)
        self.set_number(n)


if __name__ == "__main__":
    dac = MCP4725(5.11, 0x61, True)

    try:
        while True:
            try:
                v = float(input("Введите напряжение в Вольтах: "))
                dac.set_voltage(v)

            except ValueError:
                print("Вы ввели не число. Попробуйте ещё раз\n")

    except KeyboardInterrupt:
        pass

    finally:
        dac.deinit()
