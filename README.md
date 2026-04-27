import time
import r2r_adc as adc
import adc_plot as ap


mx = 3.18
dt = 0.0001
tm = 3.0

vs = []
ts = []


a = adc.R2R_ADC(mx, dt, False)

try:
    t0 = time.time()

    while time.time() - t0 < tm:
        v = a.get_sar_voltage()
        t = time.time() - t0

        vs.append(v)
        ts.append(t)

    ap.plot_voltage_vs_time(ts, vs, mx)

finally:
    a.deinit()
