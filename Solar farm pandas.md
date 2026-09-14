import pandas as pd


class SolarFarmAnalyzer:

    def __init__(self):
        pass

    # 1. Create Production DataFrame
    def create_production_df(
        self,
        data: list
    ) -> pd.DataFrame:

        df = pd.DataFrame(
            data,
            columns=[
                "TurbineID",
                "Date",
                "Energy",
                "WindSpeed",
                "OutageMinutes"
            ]
        )

        return df

    # 2. Total Energy per Turbine
    def total_energy_per_turbine(
        self,
        df: pd.DataFrame
    ) -> pd.DataFrame:

        result = (
            df.groupby("TurbineID")["Energy"]
            .sum()
            .reset_index()
        )

        result = result.rename(
            columns={"Energy": "TotalEnergy"}
        )

        return result

    # 3. Energy per Uptime Minute
    def add_energy_per_min(
        self,
        df: pd.DataFrame
    ) -> pd.DataFrame:

        df["ActiveMinutes"] = (
            1440 - df["OutageMinutes"]
        )

        df["EnergyPerMin"] = (
            df["Energy"] / df["ActiveMinutes"]
        ).round(2)

        return df

    # 4. Categorize Wind Band
    def categorize_wind_band(
        self,
        df: pd.DataFrame
    ) -> pd.DataFrame:

        df["WindBand"] = "Low"

        df.loc[
            df["WindSpeed"] >= 3,
            "WindBand"
        ] = "Moderate"

        df.loc[
            df["WindSpeed"] >= 7,
            "WindBand"
        ] = "High"

        return df

    # 5. Frequent Outage Rows
    def frequent_outage_rows(
        self,
        df: pd.DataFrame,
        n: int
    ) -> pd.DataFrame:

        result = df[
            df["OutageMinutes"] > n
        ]

        return result

    # 6. Clean and Top Production Days
    def clean_and_top_days(
        self,
        df: pd.DataFrame
    ) -> pd.DataFrame:

        result = df.dropna()

        result = result.sort_values(
            "Energy",
            ascending=False
        )

        return result
