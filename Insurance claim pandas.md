import pandas as pd


class ClaimAnalyzer:

    def __init__(self):
        pass

    # 1. Create Claims DataFrame
    def create_claims_df(self, claim_data: list) -> pd.DataFrame:

        df = pd.DataFrame(
            claim_data,
            columns=[
                "CustomerID",
                "Category",
                "Amount",
                "Status",
                "Date"
            ]
        )

        return df

    # 2. Approval Rate by Category
    def approval_rate_by_category(
        self,
        df: pd.DataFrame
    ) -> pd.DataFrame:

        total = df.groupby("Category").size()

        approved = (
            df[df["Status"] == "Approved"]
            .groupby("Category")
            .size()
        )

        result = pd.DataFrame({
            "Category": total.index,
            "Approval Rate": (
                approved / total * 100
            ).fillna(0).values
        })

        return result.reset_index(drop=True)

    # 3. Add High Amount Flag
    def add_flag_high_amount(
        self,
        df: pd.DataFrame,
        threshold: float
    ) -> pd.DataFrame:

        df["IsHighValue"] = df["Amount"] > threshold

        return df

    # 4. Get Top Pending Claims
    def get_top_pending_claims(
        self,
        df: pd.DataFrame,
        n: int
    ) -> pd.DataFrame:

        result = df[df["Status"] == "Pending"]

        result = result.sort_values(
            "Amount",
            ascending=False
        )

        return result.head(n)

    # 5. Claims Summary by Status
    def claim_summary_by_status(
        self,
        df: pd.DataFrame
    ) -> pd.DataFrame:

        result = (
            df.groupby("Status")["Amount"]
            .agg(["sum", "min", "max", "mean"])
            .reset_index()
        )

        result.columns = [
            "Status",
            "sum",
            "min",
            "max",
            "avg"
        ]

        return result
