import numpy as np


# 1. Create AQI Array
def create_aqi_array(values: list) -> np.ndarray:

    arr = np.array(values, dtype=np.int64)

    return arr


# 2. Validate AQI Array
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
    maximum = np.max(arr)
    minimum = np.min(arr)

    return average, std, maximum, minimum


# 4. Categorize AQI
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

    max_streak = 0
    current_streak = 0

    for value in arr:

        if value >= 151:
            current_streak += 1

            if current_streak > max_streak:
                max_streak = current_streak

        else:
            current_streak = 0

    return max_streak
