import mcp4725_driver as mcp
import signal_generator as sg
import time


amp = 5.0
freq = 10
fs = 1000
vmax = 5.11

if amp > vmax:
    raise ValueError("Амплитуда сигнала больше динамического диапазона 12-bit DAC")


try:
    dac = mcp.MCP4725(vmax, 0x61, False)

    t0 = time.perf_counter()

    while True:
        t = time.perf_counter() - t0
        v = amp * sg.get_sin_wave_amplitude(freq, t)

        dac.set_voltage(v)
        sg.wait_for_sampling_period(fs)

except KeyboardInterrupt:
    pass

finally:
    dac.deinit()
