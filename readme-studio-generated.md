## Description
# Live Music Circuit (LMC) Relational Database

## Team Members
* [cite_start]**Angelis Aponte** - Group Leader, SQL Writer, Data Wrangler [cite: 2]
* [cite_start]**Colby Broome** - Conceptual Modeler, Database Designer [cite: 2]

## Project Description
[cite_start]This project involves the creation of a conceptual model and a fully relational database for the company **Live Music Circuit (LMC)**[cite: 4]. [cite_start]LMC operates by coordinating between physical Venues and performing Artists[cite: 6]. [cite_start]The company manages everything from staffing via Staff coordinators to physical logistics through Resource Allocation[cite: 6]. 

[cite_start]The central entity in this data model is the **Show**, which represents a specific performance event held at a partner venue[cite: 5]. 

### Project Extension
[cite_start]To improve the business model, we extended the database to include a comprehensive ticketing and customer management system[cite: 7]. [cite_start]This extension introduces the `Customer`, `Ticket Purchase`, and `Tickets` entities[cite: 8]. [cite_start]This allows LMC to track individual audience members, manage different ticket tiers (e.g., General Admission vs. VIP), and record financial transactions linked directly to specific show events, providing deeper insight into the commercial success of each performance[cite: 8].

## Data Model: Entity Relationships
[cite_start]The Live Music Circuit database is structured to manage the logistics of live performances and production resources[cite: 10]. The core relationships are defined as follows:

* [cite_start]**Venue and Show:** A Venue can host many Shows over time, but each Show is associated with exactly one Venue[cite: 12].
* [cite_start]**Staff and Show:** Each Show is managed by one primary Staff member (coordinator) and one backup Staff member[cite: 13]. [cite_start]A single Staff member may coordinate or back up multiple Shows[cite: 14].
* [cite_start]**Venue Affiliations:** A Venue can have multiple Venue Affiliations with other venues (e.g., shared ownership or equipment)[cite: 15]. [cite_start]This is a self-referencing relationship where one venue is linked to another through an affiliation record[cite: 16].
* [cite_start]**Show and Booking:** A Show typically features multiple Bookings (e.g., an opener and a headliner), but each Booking belongs to only one specific Show[cite: 17].
* [cite_start]**Artists and Booking:** An Artist can have many Bookings across different shows, while each Booking record identifies one specific Artist[cite: 18].
* [cite_start]**Resource Allocation:** Resource Items (physical gear) are assigned to Shows through the Resource Allocation entity[cite: 19]. [cite_start]A Show can require multiple items, and an individual Resource Item can be allocated to many shows over its lifespan, provided the times do not overlap[cite: 20, 21].
* [cite_start]**Resource Type and Item:** Each Resource Type (e.g., PA System) can have many individual physical Resource Items (inventory units) associated with it[cite: 23].
* [cite_start]**Show and Tickets (Extension):** Each Show can have multiple Tickets available across different tiers (e.g., VIP, GA), but each ticket record is tied to a specific Show[cite: 24].
* [cite_start]**Ticket Purchase and Customer (Extension):** A Customer can make multiple Ticket Purchases[cite: 26]. [cite_start]Each Ticket Purchase links a specific Customer to a unique Ticket number[cite: 27].

## Database Schema (Data Dictionary Overview)
The database consists of the following core tables:

1. [cite_start]**`SHOW`**: Stores event details like date, time, age restrictions, and admission costs[cite: 25, 35, 38, 40].
2. [cite_start]**`VENUE`**: Stores physical partner locations including capacity, location, and status[cite: 56, 68].
3. [cite_start]**`STAFF`**: Contains LMC employee records for coordination purposes[cite: 68, 69].
4. [cite_start]**`ARTISTS`**: Stores performer profiles and contact information[cite: 70, 71].
5. [cite_start]**`BOOKING`**: Bridge table linking artists to specific shows and outlining their slot/format[cite: 72, 81, 84].
6. [cite_start]**`RESOURCE_TYPE`**: Categorizes the types of production equipment LMC owns and standard rental rates[cite: 95, 96, 97].
7. [cite_start]**`resource_item`**: Tracks individual, physical pieces of gear and their condition[cite: 98, 99].
8. [cite_start]**`resource_allocation`**: Transaction table logging the load-in/load-out times of specific gear for specific shows[cite: 100, 101].
9. [cite_start]**`VENUE_AFFILIATION`**: Self-referencing table tracking relationships and shared ownership between different venues[cite: 102, 103].
10. [cite_start]**`CUSTOMER`**: (Extension) Stores fan profiles, contact info, and addresses[cite: 104, 105].
11. [cite_start]**`TICKETS`**: (Extension) Represents individual generated tickets with price and tier data[cite: 106, 108].
12. [cite_start]**`TICKET_PURCHASE`**: (Extension) Transaction table linking a fan to the specific tickets they bought[cite: 109, 110].
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
| venue_phone | Primary contact number for the venue | INT | |
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

### CUSTOMER
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

### TICKETS
| Column Name | Description | Data Type | Key? |
| :--- | :--- | :--- | :--- |
| ticket_number | Unique identifier for an individual ticket | INT | Primary |
| ticket_price | Cost of the specific ticket | DECIMAL(10,2) | |
| ticket_tier | Category of ticket (e.g., VIP, GA) | VARCHAR(45) | |
| show_show_id | ID of the show this ticket is valid for | INT | Foreign |

### TICKET_PURCHASE
| Column Name | Description | Data Type | Key? |
| :--- | :--- | :--- | :--- |
| purchase_number | Unique transaction ID for a ticket sale | INT | Primary |
| tickets_ticket_number | ID of the ticket being purchased | INT | Foreign |
| customer_customer_id | ID of the customer making the purchase | INT | Foreign |

