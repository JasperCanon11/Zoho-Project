Zoho Automation System Overview

Project Description:
Developed internal business applications using Zoho Creator for the Sales team, focusing on travel itinerary management and cash advance workflows. Implemented automation logic and integrated reports into Zoho CRM using Zoho Flow for centralized data management.

Core Modules:
- Travel Itinerary Expense
- Cash Advance Management

Key Features:
- Sales users can request cash advance budgets for upcoming travel
- Daily expense logging for travel itinerary and liquidation tracking
- Automated workflow processing using Deluge scripting
- Structured data recording for accurate reporting
- Integration with Zoho CRM via Zoho Flow for centralized reporting

Technologies Used:
- Zoho Creator
- Zoho CRM
- Zoho Flow
- Deluge Script

Sample Deluge Script in Travel Itinerary Computation

// Subtotal computation
row.Subtotal=ifnull(row.TRANSPO_w_Ticket,0) + ifnull(row.TRANSPO_w_o_Ticket,0) + ifnull(row.Parking_Toll_Fee,0) + ifnull(row.Meal,0) + ifnull(row.Hotel,0) + ifnull(row.Representation,0) + ifnull(row.Miscellaneous_Amount,0) + ifnull(row.Gas_Amount,0);
// Recompute total
total_cash = 0;
for each  eachRow in input.Itinerary_Cash
{
	total_cash = total_cash + ifnull(eachRow.Subtotal,0);
}
input.Total_of_Cash = total_cash;
// Update Grandtotal with current Total_Credit_Card
input.Grandtotal = total_cash + ifnull(input.Total_Credit_Card,0);
// Total of Cash = Total Cash Expense 
if(input.Total_of_Cash != null)
{
	input.Total_Cash_Expense = input.Total_of_Cash;
}


Sample Deluge Script in Cash Advance Computation

row_total = 0;
for each  rec in input.Proposed_Itinerary
{
	row_total = row_total + ifnull(rec.Total1,0);
}
hotel_cash_total = 0;
for each  rec1 in input.Hotel
{
	if(rec1.Mode == "Cash" && rec1.Hotel_amount != null)
	{
		hotel_cash_total = hotel_cash_total + rec1.Hotel_amount;
	}
}
input.Grand_total = row_total + hotel_cash_total;
input.CA_Amount = input.Grand_total;

