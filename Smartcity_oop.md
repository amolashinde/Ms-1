class TrafficControlSystem:

    def __init__(self):
        self.traffic_data = {}

    # 1. Add Intersection
    def add_intersection(
        self,
        intersection: str,
        vehicle_count: int
    ) -> dict:

        self.traffic_data[intersection] = vehicle_count

        return self.traffic_data

    # 2. Update Vehicle Count
    def update_vehicle_count(
        self,
        intersection: str,
        new_count: int
    ) -> dict:

        if intersection not in self.traffic_data:
            return "Error: Intersection not found"

        self.traffic_data[intersection] = new_count

        return self.traffic_data

    # 3. Get Congested Intersections
    def get_congested_intersections(
        self,
        congestion_threshold: int
    ) -> dict:

        result = {}

        for intersection, count in self.traffic_data.items():

            if count > congestion_threshold:
                result[intersection] = count

        return result

    # 4. Adjust Traffic Signals
    def adjust_traffic_signals(self) -> dict:

        result = {}

        for intersection, count in self.traffic_data.items():

            if count > 80:
                result[intersection] = "Long Green"

            elif count >= 40:
                result[intersection] = "Normal Green"

            else:
                result[intersection] = "Short Green"

        return result
