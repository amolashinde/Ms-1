import numpy as np


class SensorAnalyzer:

    # 1. Create Sensor Array
    def create_sensor_array(
        self,
        sensor_values: list
    ) -> np.ndarray:

        return np.array(sensor_values, dtype=float)


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
        average = round(np.mean(sensor_array), 1)
        maximum = np.max(sensor_array)

        return (total, average, maximum)


    # 4. Filter Extreme Readings
    def filter_extreme_readings(
        self,
        sensor_array: np.ndarray
    ) -> np.ndarray:

        result = sensor_array.astype(float).copy()

        result[result >= 50.0] = result[result >= 50.0] * 0.90

        return result


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

        return np.array(
            [f"{x:.2f} units" for x in sensor_array]
        )


# -------------------------
# Example
# -------------------------

obj = SensorAnalyzer()

arr = obj.create_sensor_array(
    [28.5, 415.0, 30.7, 92.1]
)

print(arr)

print(
    obj.validate_sensor_array(
        np.array([923, -40, 810])
    )
)

print(
    obj.compute_sensor_statistics(
        np.array([20.0, 60.0, 40.0])
    )
)

print(
    obj.filter_extreme_readings(
        np.array([100.0, 50.0, 60.0])
    )
)

print(
    obj.label_high_sensors(
        np.array([100.0, 50.0, 60.0])
    )
)

print(
    obj.format_sensor_readings(
        np.array([55.0, 66.0])
    )
)

Expected Output

[ 28.5 415.   30.7  92.1]

False

(120.0, 40.0, 60.0)

[90. 45. 54.]

['High' 'Normal' 'Normal']

['55.00 units' '66.00 units']

Easy Explanation

1. Create sensor array

The PDF requires a NumPy array with "float" values.

np.array(sensor_values, dtype=float)

2. Validate

Requirements:

- Array should not be empty.
- All values should be positive.

np.all(sensor_array > 0)

3. Statistics

Use:

np.sum()
np.mean()
np.max()

Return:

(total, average, maximum)

Average is rounded to 1 decimal place.

4. Filter extreme readings

The PDF says readings ≥ 50.0 get a 10% reduction.

So:

100 × 0.90 = 90
50 × 0.90 = 45
60 × 0.90 = 54

5. Label high sensors

First calculate the mean.

Then:

value > mean → "High"
value <= mean → "Normal"

6. Format readings

Convert every value into:

55.00 units
66.00 units

The PDF specifies two decimal places followed by "" units"".





import numpy as np


# 1. Create AQI Array
def create_aqi_array(values: list) -> np.ndarray:
    return np.array(values, dtype=np.int64)


# 2. Validate AQI Data
def validate_aqi_array(arr: np.ndarray) -> bool:

    if arr.size == 0:
        return False

    if np.all((arr >= 0) & (arr <= 100)):
        return True

    return False


# 3. Compute AQI Statistics
def compute_aqi_stats(arr: np.ndarray) -> tuple:

    average = round(np.mean(arr), 2)
    std = round(np.std(arr), 2)
    maximum = round(np.max(arr), 2)
    minimum = round(np.min(arr), 2)

    return (average, std, maximum, minimum)


# 4. Categorize AQI Levels
def categorize_aqi(arr: np.ndarray) -> np.ndarray:

    result = np.where(
        arr <= 50,
        "Good",
        np.where(
            arr <= 100,
            "Moderate",
            np.where(
                arr <= 150,
                "USG",
                np.where(
                    arr <= 200,
                    "Unhealthy",
                    np.where(
                        arr <= 300,
                        "Very Unhealthy",
                        "Hazardous"
                    )
                )
            )
        )
    )

    return result


# 5. Longest Unhealthy Streak
def longest_unhealthy_streak(arr: np.ndarray) -> int:

    unhealthy = arr >= 151

    max_streak = 0
    current_streak = 0

    for value in unhealthy:

        if value:
            current_streak += 1

            if current_streak > max_streak:
                max_streak = current_streak
        else:
            current_streak = 0

    return max_streak


# -------------------------
# Example
# -------------------------

arr = create_aqi_array([32, 95, 172, 325])

print(arr)

print(validate_aqi_array(np.array([80, -2])))

print(compute_aqi_stats(
    np.array([52, 95, 172, 325])
))

print(categorize_aqi(
    np.array([32, 95, 172, 325])
))

print(longest_unhealthy_streak(
    np.array([130, 170, 180, 150, 165, 168])
))

Expected Output

[ 32  95 172 325]

False

(161.0, 103.99, 325, 52)

['Good' 'Moderate' 'Unhealthy' 'Hazardous']

2

Easy Explanation

1. Create array

np.array(values, dtype=np.int64)

Converts Python list into NumPy integer array.

2. Validate

The PDF says the array must be non-empty and values must be between "0" and "100".

np.all((arr >= 0) & (arr <= 100))

Checks every value.

3. Statistics

np.mean(arr)
np.std(arr)
np.max(arr)
np.min(arr)

Calculate:

Average
Standard deviation
Maximum
Minimum

The PDF requires rounding to 2 decimals.

4. Categorize

Remember:

0–50       Good
51–100     Moderate
101–150    USG
151–200    Unhealthy
201–300    Very Unhealthy
301–500    Hazardous

The question specifically asks for nested "np.where".

5. Longest unhealthy streak

Unhealthy means:

arr >= 151

Then count consecutive "True" values.


