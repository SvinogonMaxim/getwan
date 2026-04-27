def plot_sampling_period_hist(t):
    dt = []

    for i in range(1, len(t)):
        dt.append(t[i] - t[i - 1])

    plt.figure(figsize=(10, 6))
    plt.hist(dt)

    plt.title("Распределение периодов дискретизации измерений\nпо времени на одно измерение")
    plt.xlabel("Период измерения, с")
    plt.ylabel("Количество измерений")

    plt.xlim(0, 0.06)

    plt.grid(True)
    plt.show()
