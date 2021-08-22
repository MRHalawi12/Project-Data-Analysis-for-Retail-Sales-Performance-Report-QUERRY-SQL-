# Project-Data-Analysis-for-Retail-Sales-Performance-Report-QUERRY-SQL-
Querry of my SQL Project

Dari data yang sudah diberikan, dari pihak manajemen DQLab store ingin mengetahui:

1A. Overall perofrmance DQLab Store dari tahun 2009 - 2012 untuk jumlah order dan total sales order finished

1B. Overall performance DQLab by subcategory product yang akan dibandingkan antara tahun 2011 dan tahun 2012

 

2A. Efektifitas dan efisiensi promosi yang dilakukan selama ini, dengan menghitung burn rate dari promosi yang dilakukan overall berdasarkan tahun

2B. Efektifitas dan efisiensi promosi yang dilakukan selama ini, dengan menghitung burn rate dari promosi yang dilakukan overall berdasarkan sub-category

 

Setelah melihat hasil analisa di Sub Bab 1 dan 2, selanjutnya dilakukan analisa terhadap customer DQLab. Analisa dari sisi customer dengan menggunakan metrics:

3A. Analisa terhadap customer setiap tahunnya

3B. Analisa terhadap jumlah customer baru setiap tahunnya

3C. Cohort untuk mengetahui angka retention customer tahun 2009

.
Query for find Overall Performance by Year:
SELECT YEAR(order_date) years,
       SUM(sales) sales,
       COUNT(order_status) 'number of order'
FROM dqlab_sales_store
WHERE order_status = 'Order Finished'
GROUP BY 1;

- Querry for find Overall Performance by Product Sub Category
SELECT YEAR(order_date) AS years, product_sub_category, SUM(sales) AS sales
FROM dqlab_sales_store
WHERE order_status = 'order finished' AND YEAR(order_date) >= 2011
GROUP BY years, product_sub_category
ORDER BY years, sales DESC;

- Querry for Promotion Effectiveness and Efficiency by Years
SELECT YEAR(order_date) AS years, SUM(sales) AS sales,
SUM(discount_value) AS promotion_value, 
ROUND(SUM(discount_value)*100/SUM(sales),2) AS burn_rate_percentage
FROM dqlab_sales_store
WHERE order_status = 'order finished'
GROUP BY years;

- Querry for Promotion Effectiveness and Efficiency by Product Sub Category
SELECT YEAR(order_date) AS years, product_sub_category, product_category, 
SUM(sales) AS sales, SUM(discount_value) AS promotion_value, 
ROUND((SUM(discount_value)/SUM(sales))*100, 2) AS burn_rate_percentage
FROM dqlab_sales_store
WHERE order_status = 'order finished' AND YEAR(order_date) = 2012
GROUP BY 1,2,3
ORDER BY sales DESC;

- Querry for Customers Transactions per Year
SELECT YEAR(order_date) AS years, 
COUNT(DISTINCT customer) number_of_customer
FROM dqlab_sales_store
WHERE order_status = 'order finished'
GROUP BY years;
