import pwm_dac as pwm
import signal_generator as sg
import time


amp = 2.7
freq = 10
fs = 1000
vmax = 3.29
fpwm = 20000

if amp > vmax:
    raise ValueError("Амплитуда сигнала больше динамического диапазона PWM DAC")


try:
    dac = pwm.PWM_DAC(12, fpwm, vmax, False)

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
