# Live Music Circuit (LMC) Database Project
**Course:** MIST 4610 

## Team Members
* **Angelis Aponte** - SQL Writer, Data Wrangler- @AngelisAponte
* **Colby Broome** - Conceptual Modeler, Database Designer - @colbybroome

* ## Database Information
* **Database Name:** `mb_A4` 
* **Stored Procedures:** All 11 queries for this project have been bookmarked as stored procedures on the MySQL server as GP_Q#

---

## Problem Description
The Live Music Circuit (LMC) is an Athens, Georgia-based company that coordinates live performances, tour routing, and production logistics for local and touring acts. The central entity in this model is the **Show**, which represents a specific performance event hosted at a partner **Venue**. LMC manages the complex interactions between physical venues and performing **Artists**, ensuring each event is properly staffed by **Staff** members acting as primary and backup coordinators. 

LMC’s operations include tracking **Venue Affiliations** to manage shared ownership or equipment between locations. To support these shows, LMC manages a pool of production gear (PA systems, lighting, etc.) classified by **Resource Type**, with specific physical units tracked as **Resource Items**. These items are assigned to events through a **Resource Allocation** system that monitors load-in and load-out times.

**Extension:** To enhance the business model, we have extended the database to include a comprehensive ticketing and customer management system. This extension adds **Customer**, **Ticket Purchase**, and **Tickets** entities. This allows LMC to track individual audience members, manage different ticket tiers (e.g., General Admission vs. VIP), and record financial transactions linked directly to specific show events, providing deeper insight into the commercial success of each performance.

---

## Data Model

*(Insert your PNG image here on GitHub by dragging and dropping it into the editor)*
`![LMC Data Model](link_to_your_image.png)`

### Entity Relationship Explanation
The Live Music Circuit database is structured to manage the logistics of live performances and production resources. The relationships are defined as follows:

* **Venue and Show:** A Venue can host many Shows over time, but each Show is associated with exactly one Venue.
* **Staff and Show:** Each Show is managed by one primary Staff member (coordinator) and one backup Staff member. A single Staff member may coordinate or back up multiple Shows.
* **Venue Affiliations:** A Venue can have multiple Venue Affiliations with other venues (e.g., shared ownership or equipment). This is a self-referencing relationship where one venue is linked to another through an affiliation record.
* **Show and Booking:** A Show typically features multiple Bookings (e.g., an opener and a headliner), but each Booking belongs to only one specific Show.
* **Artists and Booking:** An Artist can have many Bookings across different shows, while each Booking record identifies one specific Artist.
* **Resource Allocation:** Resource Items (physical gear) are assigned to Shows through the Resource Allocation entity. A Show can require multiple items, and an individual Resource Item can be allocated to many shows over its lifespan, provided the times do not overlap.
* **Resource Type and Item:** Each Resource Type (e.g., PA System) can have many individual physical Resource Items (inventory units) associated with it.
* **Show and Tickets (Extension):** Each Show can have multiple Tickets available across different tiers (e.g., VIP, GA), but each ticket record is tied to a specific Show.
* **Ticket Purchase and Customer (Extension):** A Customer can make multiple Ticket Purchases. Each Ticket Purchase links a specific Customer to a unique Ticket number.

---

## Data Dictionary

### SHOW
| Column Name | Description | Data Type | Key? |
| :--- | :--- | :--- | :--- |
| show_id | Unique, identifying number for each show | INT | Primary |
| show_date_time | The date and time when a show occurs | DATETIME | |
| show_age_restriction | The youngest age allowed at show | INT | |
| show_admission | The cost of entry | VARCHAR(45) | |
| venue_venue_id | ID of the venue hosting the show | INT | Foreign |
| coordinator_id | ID of the primary staff managing the show | INT | Foreign |
| backup_coordinator_id | ID of the backup staff assigned to the show | INT | Foreign |

### VENUE
| Column Name | Description | Data Type | Key? |
| :--- | :--- | :--- | :--- |
| venue_id | Unique identifier for each physical venue | INT | Primary |
| venue_name | The official name of the performance space | VARCHAR(45) | |
| venue_phone | Primary contact number for the venue | VARCHAR(45) | |
| venue_cap | Maximum audience capacity of the space | VARCHAR(45) | |
| venue_age | Default minimum age requirement for entry | INT | |
| venue_status | Indicates if the venue is currently an LMC partner | TINYINT(1) | |
| venue_address | Street number for the venue location | INT | |
| venue_street | Street name for the venue location | VARCHAR(45) | |
| venue_city | City where the venue is located (e.g., Athens) | VARCHAR(45) | |
| venue_state | State where the venue is located (e.g., GA) | VARCHAR(45) | |

### STAFF
| Column Name | Description | Data Type | Key? |
| :--- | :--- | :--- | :--- |
| employee_id | Unique identification number for LMC staff | INT | Primary |
| employee_fname | Staff member's first name | VARCHAR(45) | |
| employee_lname | Staff member's last name | VARCHAR(45) | |

### ARTISTS
| Column Name | Description | Data Type | Key? |
| :--- | :--- | :--- | :--- |
| artist_id | Unique identifier for the performer or band | INT | Primary |
| artist_name | The stage name of the artist | VARCHAR(45) | |
| artist_phone | Primary contact phone for the artist | VARCHAR(45) | |
| artist_home_city | The city where the artist is based | VARCHAR(45) | |

### BOOKING
| Column Name | Description | Data Type | Key? |
| :--- | :--- | :--- | :--- |
| book_id | Unique identifier for a specific artist's slot | INT | Primary |
| book_format | The type of set (e.g., Acoustic, Full Band) | VARCHAR(45) | |
| book_slot_order | Performance order (e.g., 1 for opener) | INT | |
| show_show_id | ID of the show the artist is booked for | INT | Foreign |
| artists_artist_id | ID of the artist being booked | INT | Foreign |

### RESOURCE_TYPE
| Column Name | Description | Data Type | Key? |
| :--- | :--- | :--- | :--- |
| item_nm | Unique identifier for a gear category | INT | Primary |
| type_name | Name of the equipment type (e.g., PA System) | VARCHAR(45) | |
| partner_rate | Billing rate for LMC partner venues | DECIMAL(10,2) | |
| nonpartner_rate | Billing rate for non-partner venues | DECIMAL(10,2) | |

### RESOURCE_ITEM
| Column Name | Description | Data Type | Key? |
| :--- | :--- | :--- | :--- |
| inv_number | Unique physical inventory number for gear | INT | Primary |
| inv_condition | Rating of item quality (Excellent, Good, Fair) | VARCHAR(45) | |
| resource_type_item_nm | Category ID this physical item belongs to | INT | Foreign |

### RESOURCE_ALLOCATION
| Column Name | Description | Data Type | Key? |
| :--- | :--- | :--- | :--- |
| allo_transaction_number | Unique ID for the gear rental period | INT | Primary |
| allo_load_in | Planned time for gear setup | DATETIME | |
| allo_load_out | Planned time for gear removal | DATETIME | |
| resource_item_inv_number | ID of the specific gear being used | INT | Foreign |
| show_show_id | ID of the show or event using the gear | INT | Foreign |

### VENUE_AFFILIATION
| Column Name | Description | Data Type | Key? |
| :--- | :--- | :--- | :--- |
| aff_type | The nature of the link (e.g., shared ownership) | INT | |
| aff_start_date | The date the affiliation began | DATE | |
| aff_end_date | The date the affiliation ended (if applicable) | DATE | |
| venue_venue_id | ID of the first venue in the relationship | INT | Foreign |
| venue2_id | ID of the second venue in the relationship | INT | Foreign |

### CUSTOMER (Extension)
| Column Name | Description | Data Type | Key? |
| :--- | :--- | :--- | :--- |
| customer_id | Unique identifier for a music fan | INT | Primary |
| customer_fname | Customer's first name | VARCHAR(45) | |
| customer_lname | Customer's last name | VARCHAR(45) | |
| customer_email | Contact email for ticket delivery | VARCHAR(45) | |
| customer_phone | Contact phone number for the customer | VARCHAR(45) | |
| customer_adress | Street number of customer's residence | INT | |
| customer_street | Street name of customer's residence | VARCHAR(45) | |
| customer_city | City of customer's residence | VARCHAR(45) | |
| customer_state | State of customer's residence | VARCHAR(45) | |

### TICKETS (Extension)
| Column Name | Description | Data Type | Key? |
| :--- | :--- | :--- | :--- |
| ticket_number | Unique identifier for an individual ticket | INT | Primary |
| ticket_price | Cost of the specific ticket | DECIMAL(10,2) | |
| ticket_tier | Category of ticket (e.g., VIP, GA) | VARCHAR(45) | |
| show_show_id | ID of the show this ticket is valid for | INT | Foreign |

### TICKET_PURCHASE (Extension)
| Column Name | Description | Data Type | Key? |
| :--- | :--- | :--- | :--- |
| purchase_number | Unique transaction ID for a ticket sale | INT | Primary |
| tickets_ticket_number | ID of the ticket being purchased | INT | Foreign |
| customer_customer_id | ID of the customer making the purchase | INT | Foreign |

---

## Queries

### Simple Queries

**1.** List the artist name and their home city for all artists with names that start with the letter “E”. Order the results in descending order.

<img width="625" height="196" alt="image" src="https://github.com/user-attachments/assets/099dc4c9-c53a-4fcb-9848-240b84439793" />

Query 1 allows the firm to determine which artists under their management have names starting with "E" and where their home cities are located. The firm may use this information to organize their talent roster for specific alphabetical promotional materials or localized marketing campaigns, creating a more structured and targeted approach to regional promotions.

**2.** List the venue IDs that have an age policy requiring attendees to be at least 18.

<img width="629" height="416" alt="image" src="https://github.com/user-attachments/assets/02757f48-aaf2-46c0-86fd-7b4a9105444f" />

Query 2 allows the firm to determine which venues restrict attendance strictly to adults (18 and over). The firm may use this information to ensure that artists with mature content are only booked at these specific venues, creating a legally compliant touring schedule and avoiding potential fines or brand damage.

**3.** List the names and home cities of artists who are not from "Athens".

<img width="628" height="409" alt="image" src="https://github.com/user-attachments/assets/154b88a1-02ff-417f-bf9b-1bea7a24636e" />

Query 3 allows the firm to determine which artists are based outside of their immediate local market (Athens). The firm may use this information to better plan out-of-town touring schedules and accurately allocate travel budgets, creating more precise financial forecasts for tour support and travel expenses.

**4.** Write a SQL statement that reports the total number of artists within the first 20 book slots and that are originally from Atlanta or Chicago.

<img width="626" height="416" alt="image" src="https://github.com/user-attachments/assets/ce674beb-371d-4e31-8da5-6bf207739807" />

Query 4 allows the firm to determine which artists from major hub cities (Atlanta and Chicago) are securing premium, early booking slots (first 20 slots). The firm may use this information to focus their promotional advertising dollars on these highly-billed regional artists, creating maximized ticket sales in those urban markets by capitalizing on the artists' prominent show placement.

**5.** Find the inventory numbers of all "Microphones" that have a condition rating of "Fair".

<img width="630" height="421" alt="image" src="https://github.com/user-attachments/assets/83e0f7c7-40b3-4196-9119-505703e26fc6" />

Query 5 allows the firm to determine exactly which "Value Microphones" in their inventory are currently in "excellent" working condition. The firm may use this information to deploy these reliable, owned assets to upcoming mid-tier shows instead of renting new gear, creating significant cost savings on equipment rentals while ensuring high-quality audio.

### Complex Queries

**6.** List the age policy for each artist performance (include artist name and show ID), considering artists who have at least one booking. Order the results by age policy in ascending order.

<img width="625" height="419" alt="image" src="https://github.com/user-attachments/assets/303e0c39-dd38-4b79-814e-b64c35faa352" />

Query 6 allows the firm to determine the specific age restrictions tied to each artist’s upcoming performances. The firm may use this information to accurately segment their email marketing campaigns to the appropriate demographic, creating more efficient ad campaigns 
by preventing wasted ad spend on underage fans.

**7.** For each artist, list the artist name and the total number of shows they performed where the artist’s average tickets sold across all their bookings is at least 100. Order the results by the artist name in descending order.

<img width="620" height="419" alt="image" src="https://github.com/user-attachments/assets/5e6013f0-0866-49d5-a794-a01ba9d79e45" />

Query 7 allows the firm to determine which artists consistently draw large crowds by maintaining an average of 100 or more ticket sales per show. The firm may use this information to reward their top-performing talent with priority contract renewals and larger venue assignments, creating an incentive for artists to stay with the agency while maximizing the firm's overall revenue.

**8.** Write a SQL statement that lists the total number of tickets purchased by customers who are from Georgia. Order results by ascending price.

<img width="623" height="417" alt="image" src="https://github.com/user-attachments/assets/af8ea3a7-e3c1-4f11-a879-9ace349d973d" />

Query 8 allows the firm to determine the pricing preferences and purchasing behavior of their local customer base in Georgia. The firm may use this information to adjust future ticket pricing tiers in that specific region, creating a perfectly balanced mix of budget and VIP tickets that local fans are actually willing to buy.

**9.** List the number of shows per venue that are happening in June.

<img width="624" height="421" alt="image" src="https://github.com/user-attachments/assets/61e2a3b1-4b88-4085-8511-5a788e2b8812" />

Query 9 allows the firm to determine the booking density and utilization rate of each venue during the busy summer month of June. The firm may use this information to identify which venues have empty dates ("dark days") on their calendars to schedule last-minute summer shows, creating an opportunity to capture additional seasonal revenue.

**10.** Find artist bookings where the slot order is higher than that artist’s average slot order across all events.

<img width="1161" height="787" alt="image" src="https://github.com/user-attachments/assets/bad0a612-3744-40d8-8d49-c14dbf51eddb" />

Query 10 allows the firm to determine which artists are being booked in later, more prominent performance slots compared to their own historical averages. The firm may use this information to track an artist's rising popularity and momentum, creating the data-driven leverage needed for management to negotiate higher performance fees and better billing for their growing stars.

**11.** List artists who have never been booked for a show.

<img width="632" height="406" alt="image" src="https://github.com/user-attachments/assets/43cef716-555a-46ca-a592-7d36c3205c6a" />

Query 11 allows the firm to determine which represented artists currently have zero scheduled bookings. The firm may use this information to identify dormant talent on their roster to either launch new promotional pushes or drop them from the agency, creating a more efficient, active, and profitable talent pool.

## Query Features Matrix

The following table indicates which advanced SQL features are utilized in each of our queries to ensure comprehensive coverage of database operations:
| SQL Feature | Query 1 | Query 2 | Query 3 | Query 4 | Query 5 | Query 6 | Query 7 | Query 8 | Query 9 | Query 10 | Query 11 |
| :--- | :---: | :---: | :---: | :---: | :---: | :---: | :---: | :---: | :---: | :---: | :---: |
| **Multiple Joins** | | | | | | X | X | X | | | |
| **Subquery** | | | | | | | X | | | X | X |
| **Correlated Subquery**| | | | | | | | | | X | |
| **Group By** | | | | | | | X | X | X | | |
| **Built In Functions** | | | | X | | | X | X | X | X | |
| **Having** | | | | | | | X | | | | |
| **Exist** | | | | | | | | | | | X |
| **Order By** | X | | | | | X | X | X | | | |
