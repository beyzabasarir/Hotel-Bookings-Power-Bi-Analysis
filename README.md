# Hotel Bookings Analysis - Power BI Dashboard

<p align="center">
  <img src="https://drive.google.com/uc?id=1HHAROfbgFfQb9ers5X79Vlk0yGXvfDh5" width="30%" />
  <img src="https://drive.google.com/uc?id=1UGtlCGbvkcuu0QvWOhLsosj_ccuCfTHj" width="30%" />
  <img src="https://drive.google.com/uc?id=1fY8nkTxdzB5kctd8_IoyQLqwKvDTUxSl" width="30%" />
</p>

## Introduction

This project presents a Power BI dashboard developed to analyze hotel booking trends and provide actionable insights. The dataset used for this analysis comprises real-world hotel booking records for two types of hotels—City Hotel and Resort Hotel—over a span of two years. Through this analysis, the goal is to present a clear and concise overview of booking behaviors, identify trends, and provide meaningful insights into customer preferences and operational outcomes.

## Dataset Overview 

The dataset  contains 119,390 observations and provides insights into several booking-related variables, such as whether the booking was canceled, the lead time before arrival, and the type of deposit made. Other features capture information about customer preferences, including meal packages, room types, and the number of special requests.  It includes anonymized data to protect sensitive customer information. 

## Methodology

The analysis was structured into several stages:

### 1. Data Cleaning and Preparation

The data underwent several transformation steps to enhance its usability and align with analytical goals:

- **Converting data types** to ensure consistency, especially for date and numeric fields. 
- **Renaming columns** for better clarity (e.g., `lead_time → reservation_to_arrival_days`).
- **Removing unnecessary columns** that will not be used in the analysis, such as `agent` and `company`.  
- **Creating new variables** such as `arrival_date` by combining year, month, and day fields. 
- **Generating calculated columns** like `total_stay`, which aggregates weekend and weekday stays.

### 2. Derived Metrics & DAX Calculations

New metrics, such as "total_stay," were created to better understand guest durations. A unified "reservation_date" field was added to connect booking and arrival timelines effectively. The analysis also employed several DAX formulas to calculate key performance indicators (KPIs): 

<details>
  <summary>Click to view examples of DAX formulas </summary>

  - **Total Reservations**: The total count of all booking statuses.
    ```DAX
    Total Reservations = COUNT(hotel_bookings[reservation_status])
    ```

  - **Total Cancellations**: The number of canceled reservations.
    ```DAX
    Total Cancellations = CALCULATE(COUNTROWS(hotel_bookings), hotel_bookings[is_canceled] = 1)
    ```

  - **Total Revenue**: Calculated using the formula `average_daily_rate * total_stay`, focusing only on non-canceled bookings to reflect actual earnings.
    ```DAX
    Total Revenue = 
    CALCULATE(
    SUMX(
        FILTER(
            'hotel_bookings',
            'hotel_bookings'[average_daily_rate] > 0
        ),
        'hotel_bookings'[average_daily_rate] * 'hotel_bookings'[total_stay]
    ),
    'hotel_bookings'[is_canceled] = 0
    )
    ```

  - **Cancellation Rate**: Measured as the ratio of canceled reservations to total reservations.
    ```DAX
    Cancellation Rate = DIVIDE(
        CALCULATE(COUNTROWS(hotel_bookings), hotel_bookings[is_canceled] = 1), 
        COUNTROWS(hotel_bookings), 
        0
    )
    ```

  - **Average Lead Time**: The average time between reservation and arrival.
    ```DAX
    Average Lead Time = 
        ROUND(
            AVERAGE(hotel_bookings[reservation_to_arrival_days]), 
            0
        )
    ```

  - **Average Stay**: The mean length of stay for non-canceled bookings.
    ```DAX
    Average Stay =  
    ROUND(
        CALCULATE(
            AVERAGE('hotel_bookings'[total_stay]),
            'hotel_bookings'[is_canceled] = 0
        ),
        0
    )
    ```

  - **Processed Reservations**: The number of bookings successfully completed (not canceled).
    ```DAX
    Processed Reservations = CALCULATE(
        COUNT('hotel_bookings'[reservation_status]),
        ('hotel_bookings'[is_canceled] <> 1)
    )
    ```

  - **Total Repeated Guests**: The count of guests with multiple bookings, categorized further by loyalty status.
    ```DAX
    Total Repeated Guests = CALCULATE(
        COUNTROWS('hotel_bookings'),
        'hotel_bookings'[is_repeated_guest] = TRUE
    )
    ```

</details>

### 3. Visualizations 

Dashboards were developed to provide an intuitive overview of trends, comparisons, and performance metrics. These were grouped by key themes such as revenue trends by hotels, guest demographics and booking outcomes.  

### 4. Slicer Customization 

Interactive slicers  added to allow users to view the dataset dynamically. These slicers enable filtering by various dimensions, such as year, distribution channel, and customer type, facilitating deeper analysis tailored to specific queries. 

## Dashboard 

![Hotel Performance PowerBI Dashboard](https://github.com/beyzabasarir/Hotel-Bookings-Power-Bi-Analysis/blob/main/Hotel-Bookings-PowerBi-Dashboard.gif) 

> **Note**: The quality of this GIF may appear lower than expected. A comprehensive view of the dashboard was intended, including tooltips and slicer performance. However, due to file size limitations on GitHub, the file had to be compressed, resulting in some degradation of visual quality. For the best experience, you can download and explore the full-resolution dashboard in Power BI by accessing the [Power BI file here](https://github.com/beyzabasarir/Hotel-Bookings-Power-Bi-Analysis/blob/main/Hotel-Bookings-Power-Bi.pbix) and opening it in Power BI Desktop.

### Hotel Performance Analysis
City Hotel generates the majority of reservations and revenue, making it the more popular choice among guests. Although City Hotel has a higher number of repeated guests, Resort Hotel demonstrates stronger guest loyalty when assessed as a proportion of total reservations, potentially indicating differences in guest experiences or offerings. Seasonal trends reveal contrasting patterns: City Hotel sees a peak in daily guests from April to June, followed by a notable drop in July, while Resort Hotel maintains steady demand year-round, with slight increases in daily guest numbers during April, May, July, and August. Revenue patterns reflect these trends, with summer months generally yielding higher revenue for both hotels. Interestingly, despite hosting fewer guests overall, Resort Hotel surpasses City Hotel in revenue during July and August, although City Hotel remains the annual leader.

### Guests Analysis
The "Guests Analysis" page explores guest behavior and demographics in detail. Transient guests dominate, indicating a preference for short-term, individual stays. Summer months feature longer stays, especially among contract customers, aligning with peak travel periods. Loyalty remains an area for growth, as most guests are categorized as low-loyalty. Contract customers stand out as a high-value segment, exhibiting the longest lead times and stays despite their relatively smaller numbers. Room preferences are focused on a few key categories, with Room Type A accommodating the largest share of guests.

### Reservations Analysis
The "Reservations Analysis" page highlights significant seasonal and geographic patterns in reservations and cancellations. Summer months, particularly July, see higher cancellation rates, reflecting booking volatility during peak travel periods. Regions such as Portugal and Brazil show notably high cancellation rates, indicating opportunities for targeted strategies to reduce cancellations in these markets. Online Travel Agencies are identified as the primary channel for cancellations, suggesting the need for stronger guest retention efforts within this segment. Processed reservations peak in August, coinciding with heightened seasonal activity, emphasizing the need for efficient operations during this time.

### General Insights
The analysis uncovers key opportunities to refine operational and strategic approaches across both hotels. City Hotel’s summer performance, characterized by a guest drop in July, contrasts with Resort Hotel’s steady demand and higher revenue during the same period. This points to potential for City Hotel to optimize its mid-summer offerings to sustain performance. Resort Hotel’s ability to generate higher revenue with fewer guests during peak months suggests effective pricing or guest spending patterns that could be further leveraged. Low guest loyalty across both hotels signals the need for enhanced loyalty programs, particularly for City Hotel, which relies heavily on transient guests. Addressing high cancellation rates, particularly in regions like Portugal and Brazil and within Online Travel Agency bookings, offers a clear path to revenue retention. By focusing on seasonal optimization, strengthening loyalty strategies, and targeting key regional markets, both hotels can achieve more consistent performance and sustained growth.
