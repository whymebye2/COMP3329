```mermaid
classDiagram
    %% User Hierarchy
    class User {
        <<abstract>>
        +userId: String
        +name: String
        +email: String
        +role: Enum
    }
    
    class Student {
        +studentId: String
        +faculty: String
        +yearOfStudy: Integer
    }
    
    class CEDARSAccommodationSpecialist {
        +staffId: String
        +department: String
    }
    
    class PropertyOwner {
        +ownerId: String
        +contactInfo: String
        +bankDetails: String
    }
    
    User <|-- Student
    User <|-- CEDARSAccommodationSpecialist
    User <|-- PropertyOwner
    
    %% Core Accommodation Classes
    class Accommodation {
        +accommodationId: String
        +type: String
        +price: Float
        +availabilityStatus: Enum
        +averageRating: Float
    }
    
    class AccommodationDetails {
        +description: String
        +amenities: String[]
        +photos: Blob[]
        +houseRules: String
    }
    
    class Geolocation {
        +latitude: Float
        +longitude: Float
    }
    
    class Address {
        +buildingName: String
        +street: String
        +district: String
    }
    
    %% Reservation System
    class Reservation {
        +reservationId: String
        +startDate: Date
        +endDate: Date
        +status: Enum
    }
    
    class Contract {
        +contractId: String
        +terms: String
        +signingStatus: Enum
    }
    
    class Rating {
        +ratingId: String
        +score: Integer
        +comment: String
        +date: Date
    }
    
    %% Notification System
    class Notification {
        +notificationId: String
        +type: String
        +content: String
        +timestamp: DateTime
    }
    
    class SystemEvent {
        +eventType: Enum
        +timestamp: DateTime
    }
    
    %% Relationships
    Student --> Accommodation : "views"
    Student --> Reservation : "makes"
    Student --> Rating : "rates"
    
    CEDARSAccommodationSpecialist --> Accommodation : "manages"
    CEDARSAccommodationSpecialist --> Reservation : "processes"
    CEDARSAccommodationSpecialist --> Notification : "receives"
    
    PropertyOwner --> Accommodation : "owns"
    PropertyOwner --> Notification : "receives"
    
    Accommodation "1" *-- "1" AccommodationDetails : has
    Accommodation "1" *-- "*" Rating : has
    Accommodation "1" *-- "1" Address : has
    Address "1" *-- "1" Geolocation : contains
    
    Reservation "1" --> "1" Accommodation : for
    Reservation "1" --> "0..1" Contract : becomes
    
    SystemEvent --> Notification : "generates"
    Notification --> User : "sent to"
    Notification --> PropertyOwner : "sent to"
    
    %% Enums
    class AccommodationStatus {
        <<enumeration>>
        AVAILABLE
        RESERVED
        OCCUPIED
    }
    
    class ReservationStatus {
        <<enumeration>>
        PENDING
        CONFIRMED
        CANCELLED
    }
    
    class ContractStatus {
        <<enumeration>>
        DRAFT
        SIGNING_IN_PROGRESS
        COMPLETED
        TERMINATED
    }
    
    Accommodation --|> AccommodationStatus
    Reservation --|> ReservationStatus
    Contract --|> ContractStatus
