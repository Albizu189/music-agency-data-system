# 🎵 Music Performance Management System (MPMS)  
*A database system to manage musical groups, venues, performances, payments, equipment rentals, and planned events.*  

---

## 📊 **Database Schema (Mermaid)**  
```mermaid
erDiagram
    MusicalGroups ||--o{ Performances : "performs"
    MusicalGroups ||--o{ EquipmentRentals : "rents"
    MusicalGroups ||--o{ PlannedEvents : "plans"
    Venues ||--o{ Performances : "hosts"
    Performances ||--|| Payments : "receives"
    EquipmentSuppliers ||--o{ EquipmentRentals : "provides"

    MusicalGroups {
        int GroupID PK
        string GroupName
        string Genre
        int FormationYear
        string ContactPerson
        string Phone
        string Email
    }

    Venues {
        int VenueID PK
        string VenueName
        string Location
        int Capacity
        string Type
        string ContactPhone
        boolean IsPrivate
    }

    Performances {
        int PerformanceID PK
        int GroupID FK
        int VenueID FK
        date Date
        time StartTime
        time EndTime
        int ExpectedAudience
        string Status
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
        string Specialty
        string Contact
    }

    EquipmentRentals {
        int RentalID PK
        int SupplierID FK
        int GroupID FK
        string EquipmentType
        date RentalStartDate
        date RentalEndDate
        decimal Cost
        boolean IsReturned
    }

    PlannedEvents {
        int EventID PK
        int GroupID FK
        string EventName
        date PlannedDate
        decimal EstimatedBudget
        string Status
    }