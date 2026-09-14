import numpy as np


class SensorAnalyzer:

    # 1. Create Sensor Array
    def create_sensor_array(
        self,
        sensor_values: list
    ) -> np.ndarray:

        arr = np.array(
            sensor_values,
            dtype=float
        )

        return arr

    # 2. Validate Sensor Array
    def validate_sensor_array(
        self,
        sensor_array: np.ndarray
    ) -> bool:

        if sensor_array.size == 0:
            return False

        if np.all(sensor_array > 0):
            return True

        return False

    # 3. Compute Sensor Statistics
    def compute_sensor_statistics(
        self,
        sensor_array: np.ndarray
    ) -> tuple:

        total = np.sum(sensor_array)

        average = round(
            np.mean(sensor_array),
            1
        )

        maximum = np.max(sensor_array)

        return total, average, maximum

    # 4. Filter Extreme Readings
    def filter_extreme_readings(
        self,
        sensor_array: np.ndarray
    ) -> np.ndarray:

        arr = sensor_array.astype(float)

        arr[arr >= 50.0] = (
            arr[arr >= 50.0] * 0.90
        )

        return arr

    # 5. Label High Sensors
    def label_high_sensors(
        self,
        sensor_array: np.ndarray
    ) -> np.ndarray:

        mean_value = np.mean(sensor_array)

        result = np.where(
            sensor_array > mean_value,
            "High",
            "Normal"
        )

        return result

    # 6. Format Sensor Readings
    def format_sensor_readings(
        self,
        sensor_array: np.ndarray
    ) -> np.ndarray:

        result = np.array([
            f"{x:.2f} units"
            for x in sensor_array
        ])

        return result
