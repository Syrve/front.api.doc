---
title: Receiving front reports
layout: default
---

In API V7 it became possible to get the markup of some front reports
[`GetReportMarkupById`](https://syrve.github.io/front.api.sdk/v7/html/M_Resto_Front_Api_IOperationService_GetReportMarkupById.htm).

You can pass any of the strings specified in the list as the report identifier `reportId`

+ `"X_REPORT"` — X-report;
+ `"Z_REPORT"` — Z-report;
+ `"SALES_BY_TYPE_WITH_TAX_SERVER_REPORT"` — 011 Total revenue by type with taxes;
+ `"SERVER_HOURLY_REPORT"` — 012 Total revenue per hour;
+ `"SALES_BY_WAITER_SERVER_REPORT"` — 013 Total revenue by waiters;
+ `"ORDERS_SUMMARY_REPORT"` — 015 Brief report on open orders and sales by venue;
+ `"SALES_BY_PAYMENT_TYPE_SERVER_REPORT"` — 016 Receipts by payment type;
+ `"DISH_EXPENSE_SERVER_REPORT"` — 021 Total food consumption;
+ `"SERVER_SALES_REPORT"` — 023 Total food sales;
+ `"SERVER_WRITEOFF_REPORT"` — 024 Total write-offs of dishes;
+ `"SUMMARY_REPORT"` — 031 Consolidated report;
+ `"EMPLOYEES_NUTRITION_REPORT"` — 032 Staff meals;
+ `"ORDER_GAP_REPORT"` — 033 Time from bill to payment;
+ `"SESSION_WRITEOFF_REPORT"` — 034 Write-offs of dishes;
+ `"EMPLOYEES_ATTENDANCES_REPORT"` — 035 Employee attendance;
+ `"SESSION_DISCOUNT_REPORT"` — 036 Report on discounts and allowances;
+ `"PROBLEM_OPERATIONS_REPORT"` — 037 Dangerous operations;
+ `"EMPLOYEE_DEBTS_REPORT"` — 038 Payment to employees;
+ `"EGAIS_PRODUCT_UNSEAL_REPORT"` — 039 Report on container openings;
+ `"SALES_BY_TYPE_WITH_TAX_SESSION_REPORT"` — 041 Revenue by type with taxes;
+ `"SESSION_HOURLY_REPORT"` — 042 Revenue hourly;
+ `"SESSION_SALES_REPORT"` — 043 Sales of dishes;
+ `"DISH_EXPENSE_SESSION_REPORT"` — 044 Consumption of dishes;
+ `"CAFE_SESSION_FULL_REPORT"` — 045 Full cash register shift report;
+ `"CAFE_SESSION_SALES_REGISTER_REPORT"` — 046 Register of accounts;
+ `"SALES_BY_PAYMENT_TYPE_SESSION_REPORT"` — 047 Receipts by type of payment per shift;
+ `"CAFE_SESSION_SUMMARY_REPORT"` — 048 Total per shift;
+ `"CHEQUE_TAPE_REPORT"` — 049 Cash tape;
+ `"DELIVERY_REPORT"` — 050 Delivery report;
+ `"CAFE_SESSION_SALES_EXTENDED_REGISTER_REPORT"` — 051 Extended register of accounts;
+ `"PAY_IN_OUT_REPORT"` — 052 Report on deposits and withdrawals;
+ `"DELIVERY_DISHES_REPORT"` — 053 Dishes for preparing deliveries;
+ `"DONATIONS_REPORT"` — 054 Tips report.