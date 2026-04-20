import numpy as np
import time


def get_sin_wave_amplitude(freq, t):
    return (np.sin(2 * np.pi * freq * t) + 1) / 2


def wait_for_sampling_period(fs):
    time.sleep(1 / fs)
