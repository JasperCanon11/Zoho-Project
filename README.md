# Zoho Automation System

## Overview
Developed internal business applications using Zoho Creator for the Sales team, focusing on travel itinerary management and cash advance workflows. The system automates request processing, expense tracking, and integrates reports into Zoho CRM via Zoho Flow for centralized data management.

## Core Modules
- Travel Itinerary Expense Tracking
- Cash Advance Management

## Key Features
- Cash advance request system for travel budgets
- Daily expense logging for itinerary liquidation
- Automated workflows using Deluge scripting
- Structured data recording for accurate reporting
- Integration with Zoho CRM via Zoho Flow

## Technologies Used
- Zoho Creator
- Zoho CRM
- Zoho Flow
- Deluge Script

## Sample Logic

### Travel Itinerary Computation
```deluge
// Compute subtotal per row
row.Subtotal = ifnull(row.TRANSPO_w_Ticket,0) 
             + ifnull(row.TRANSPO_w_o_Ticket,0) 
             + ifnull(row.Parking_Toll_Fee,0) 
             + ifnull(row.Meal,0) 
             + ifnull(row.Hotel,0) 
             + ifnull(row.Representation,0) 
             + ifnull(row.Miscellaneous_Amount,0) 
             + ifnull(row.Gas_Amount,0);

// Compute total cash
total_cash = 0;
for each eachRow in input.Itinerary_Cash
{
    total_cash = total_cash + ifnull(eachRow.Subtotal,0);
}

input.Total_of_Cash = total_cash;
input.Grandtotal = total_cash + ifnull(input.Total_Credit_Card,0);

### Travel Itinerary Computation
```deluge
row_total = 0;
for each rec in input.Proposed_Itinerary
{
    row_total = row_total + ifnull(rec.Total1,0);
}

hotel_cash_total = 0;
for each rec1 in input.Hotel
{
    if(rec1.Mode == "Cash" && rec1.Hotel_amount != null)
    {
        hotel_cash_total = hotel_cash_total + rec1.Hotel_amount;
    }
}

input.Grand_total = row_total + hotel_cash_total;
input.CA_Amount = input.Grand_total;
