import pandas as pd


class ReturnsAnalyzer:

    # 1. Create Orders DataFrame
    def create_orders_df(
        self,
        order_data: list
    ) -> pd.DataFrame:

        df = pd.DataFrame(
            order_data,
            columns=[
                "OrderID",
                "SellerID",
                "Category",
                "OrderDate",
                "OrderAmount"
            ]
        )

        return df

    # 2. Create Returns DataFrame
    def create_returns_df(
        self,
        return_data: list
    ) -> pd.DataFrame:

        df = pd.DataFrame(
            return_data,
            columns=[
                "OrderID",
                "ReturnDate",
                "RefundAmount",
                "Reason"
            ]
        )

        return df

    # 3. Merge Orders and Returns
    def merge_orders_returns(
        self,
        orders_df: pd.DataFrame,
        returns_df: pd.DataFrame
    ) -> pd.DataFrame:

        result = pd.merge(
            orders_df,
            returns_df,
            on="OrderID",
            how="left"
        )

        return result

    # 4. Category-wise Refund Rate
    def category_refund_rate(
        self,
        merged_df: pd.DataFrame
    ) -> pd.DataFrame:

        merged_df["IsReturned"] = (
            merged_df["RefundAmount"].notna()
            .astype(int)
        )

        total_orders = (
            merged_df
            .groupby("Category")
            .size()
        )

        returned_orders = (
            merged_df
            .groupby("Category")["IsReturned"]
            .sum()
        )

        result = pd.DataFrame({
            "Category": total_orders.index,
            "Orders": total_orders.values,
            "ReturnedOrders": returned_orders.values,
            "RefundRate":
                (returned_orders / total_orders * 100).values
        })

        result = result.sort_values(
            "Category"
        )

        return result.reset_index(drop=True)

    # 5. High Return Sellers
    def high_return_sellers(
        self,
        merged_df: pd.DataFrame,
        n: int
    ) -> pd.DataFrame:

        returned = merged_df[
            merged_df["IsReturned"] == 1
        ]

        result = (
            returned
            .groupby("SellerID")
            .size()
            .reset_index(name="ReturnCount")
        )

        result = result[
            result["ReturnCount"] > n
        ]

        return result.reset_index(drop=True)

    # 6. Clean Return Data
    def clean_returns_data(
        self,
        returns_df: pd.DataFrame
    ) -> pd.DataFrame:

        result = returns_df.dropna(
            subset=["Reason"]
        )

        result = result[
            result["RefundAmount"].notna()
            & (result["RefundAmount"] > 0)
        ]

        return result.reset_index(drop=True)
