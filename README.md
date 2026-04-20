import r2r_dac as r2r
import signal_generator as sg
import time


amp = 3.2
freq = 10
fs = 1000
vmax = 3.3


try:
    dac = r2r.R2R_DAC([16, 20, 21, 25, 26, 17, 27, 22], vmax, False)

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
