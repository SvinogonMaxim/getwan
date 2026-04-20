import time
import math
import pwm_dac
import signal_generator


a = 1.30
f = 10
fd = 200
vmax = 3.29


try:
    dac = pwm_dac.PWM_DAC(12, 500, vmax, False)

    t0 = time.perf_counter()

    while True:
        t = time.perf_counter() - t0

        v = a + a * math.sin(2 * math.pi * f * t)

        dac.set_voltage(v)
        time.sleep(1 / fd)

finally:
    dac.deinit()
