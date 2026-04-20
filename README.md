import pwm_dac as pwm
import signal_generator as sg
import time


a = 3.0
f = 5
fs = 500
fpwm = 500
vmax = 3.3
pin = 12
t = 0


if name == "main":
    try:
        dac = pwm.PWM_DAC(pin, fpwm, vmax, False)

        while True:
            v = a * sg.get_sin_wave_amplitude(f, t)
            dac.set_voltage(v)

            sg.wait_for_sampling_period(fs)
            t += 1 / fs

    except KeyboardInterrupt:
        pass

    finally:
        dac.deinit()
