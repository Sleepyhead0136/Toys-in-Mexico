Profit by Category = 
SUMX(
    sales,
    sales[Units] * RELATED(products[Profit])
)

Sales Drop % = 
VAR InStock = [Sales When InStock]
VAR OutStock = [Sales When OutOfStock]
RETURN
IF(
    InStock > 0,
    DIVIDE(OutStock, InStock, 0),
    BLANK()


Sales When InStock = 
CALCULATE(
    [Total Sales(pieces)],
    FILTER(
        inventory,
        inventory[stock_on_hand] > 0
    )
)

Sales When OutOfStock = 
CALCULATE(
    [Total Sales(pieces)],
    FILTER(
        inventory,
        inventory[stock_on_hand] = 0
    )
)

Stock Profit (Filtered by Category) = 
VAR SelectedCategory = SELECTEDVALUE(products[Product_Category])
RETURN
IF(
    NOT ISBLANK(SelectedCategory),
    CALCULATE(
        SUMX(
            inventory,
            inventory[Stock_On_Hand] * RELATED(products[Profit])
        ),
        products[Product_Category] = SelectedCategory
    ),
    SUMX(
        inventory,
        inventory[Stock_On_Hand] * RELATED(products[Profit])
    )
)

total profit = 
SUM(products[Profit])

Total Sales(pieces) = SUM(sales[units])

Units by Selected Category = 
VAR idCategory = SELECTEDVALUE(products[Product_Category])

RETURN
CALCULATE(
    SUM(sales[Units] ),
    FILTER(
        sales,
        RELATED(products[Product_Category]) = idCategory
    )
)

Seasons = 
SWITCH(
   TRUE(),
    MONTH('calendar'[Date]) in {12,1,2}, "winter",
    MONTH('calendar'[Date]) in {3,4,5}, "spring",
    MONTH('calendar'[Date]) in {6,7,8}, "summer",
    MONTH('calendar'[Date]) in {9,10,11},"autumn",
    "other"
)
