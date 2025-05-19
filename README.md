# 🎵 Music Agency Management System (MAMS)  
*A database system to manage musical groups, venues, performances, payments and related expenses.*

---
## Problem Description
A Music Agency represents several bands and artists, which perform in several venues. Each performance demands a payment and sometimes aditional equipment that can be rented to distinct suppliers. Also, a venue can complain about some situation provocated by a band in its performance or an artist can complain about a venue. The Music Agency wants to track all payments received and made by artists and calculate their monthly contribution to the agency.

## Relational Model

**MusicalGroups** (<u>GroupID</u>, GroupName, Genre, ContactPerson, Phone, Email)  
**Venues** (<u>VenueID</u>, VenueName, Location, Capacity, BusinessType, ContactPhone)  
**Performances** (<u>PerformanceID</u>, *GroupID*, *VenueID*, Date, StartTime, EndTime, Status)  
**Payments** (<u>PaymentID</u>, *PerformanceID*, Amount, Currency, PaymentDate, PaymentMethod, IsPaid)  
**Complaints** (<u>ComplaintID</u>, *PerformanceID*, IssuerType, Description, ResolutionStatus)  
**EquipmentSuppliers** (<u>SupplierID</u>, SupplierName, Contact)  
**EquipmentTypes** (<u>TypeID</u>, Name)  
**EquipmentRentals** (<u>RentalID</u>, *SupplierID*, *PerformanceID*, *TypeID*, RentalAmount, RentalCurrency, RentalPaymentDate, RentalPaymentMethod, RentalIsPaid)

### Attribute Constraints
- **Venues.BusinessType**: `Public` or `Private`
- **Performances.Status**: `Scheduled`, `Completed`, `Cancelled`
- **Complaints.IssuerType**: `Venue` or `Group`
- **Complaints.ResolutionStatus**: `Pending`, `Resolved` or `Rejected`
- **Payments.PaymentMethod**: `Cash`, `Bank Transfer` or `Card`
- **EquipmentTypes.Name**: `Instruments`, `Lights`, `Sound`, `SceneComplements`
- **EquipmentRentals.RentalPaymentMethod**: `Cash`, `Bank Transfer` or `Card`

<!-- ### Foreign Key Constraints:
1. *(Performances)*.*GroupID* → *(MusicalGroups)*.GroupID  
2. *(Performances)*.*VenueID* → *(Venues)*.VenueID  
3. *(Payments)*.*PerformanceID* → *(Performances)*.PerformanceID  
4. *(Complaints)*.*PerformanceID* → *(Performances)*.PerformanceID  
5. *(EquipmentRentals)*.*SupplierID* → *(EquipmentSuppliers)*.SupplierID  
6. *(EquipmentRentals)*.*PerformanceID* → *(Performances)*.PerformanceID  
7. *(EquipmentRentals)*.*TypeID* → *(EquipmentTypes)*.TypeID -->

## **Database Schema (with Mermaid)**
```mermaid
erDiagram
    MusicalGroups ||--o{ Performances : "performs"
    MusicalGroups ||--o{ EquipmentRentals : "rents"
    Venues ||--o{ Performances : "hosts"
    Performances ||--|| Payments : "receives"
    Performances ||--o{ Complaints : "triggers"
    EquipmentSuppliers ||--o{ EquipmentRentals : "provides"
    EquipmentTypes ||--o{ EquipmentRentals : "categorizes"

    MusicalGroups {
        int GroupID PK
        string GroupName
        string Genre
        string ContactPerson
        string Phone
        string Email
    }

    Venues {
        int VenueID PK
        string VenueName
        string Location
        int Capacity
        string BusinessType
        string ContactPhone
    }

    Performances {
        int PerformanceID PK
        int GroupID FK
        int VenueID FK
        date Date
        time StartTime
        time EndTime
        string Status
    }

    Complaints {
        int ComplaintID PPK
        int PerformanceID PFK
        string IssuerType
        string Description
        string ResolutionStatus
    }

    Payments {
        int PaymentID PK
        int PerformanceID FK
        decimal Amount
        string Currency
        date PaymentDate
        string PaymentMethod
        boolean IsPaid
    }

    EquipmentSuppliers {
        int SupplierID PK
        string SupplierName
        string Contact
    }

    EquipmentTypes {
        int TypeID PK
        string Name
    }

    EquipmentRentals {
        int RentalID PK
        int SupplierID FK
        int PerformanceID FK
        int TypeID FK
        decimal RentalAmount
        string RentalCurrency
        date RentalPaymentDate
        string RentalPaymentMethod
        boolean RentalIsPaid
    }