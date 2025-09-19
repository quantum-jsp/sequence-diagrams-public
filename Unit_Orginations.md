```mermaid
sequenceDiagram
    participant Unit
    participant Quantum API
    participant Internal Processes
    
    autonumber

    Unit ->> Quantum API: Submit Application including Plaid Token
    Quantum API ->> Internal Processes: Retrieve Plaid Transactions
    Quantum API ->> Unit: [CWH] Status Updated to ReviewInProgress
    alt 
        Quantum API ->> Internal Processes: [CWH] Status Updated to Declined
        Internal Processes ->> Quantum API: @8pm ET Trigger AAN Sends required
        Quantum API ->> Unit: [CWH] @8pm ET Status Updated to Declined and AAN Link Included
    end 
    Quantum API ->> Unit: [CWH] Offers Available including Disclosures
    Unit ->> Quantum API: An Offer is accepted
    alt
        Unit ->> Quantum API: All Offers Declined
        Quantum API ->> Unit: [CWH] Updates Status to Offers-Declined
    end
    opt Docs Required
        loop
            Quantum API ->> Unit: [CWH] Document Required with Category
            Unit ->> Quantum API: Upload Document
        end
    end
    Quantum API ->> Unit: [CWH] Contract Available including signing links
    Quantum API ->> Internal Processes: Document Signed by all Participants
    Quantum API --> Unit: Contract Completed (updateQuantumApplicationStatusContractSigned)
    Quantum API ->> Unit: [CWH] Status updated to In-Review
    Quantum API ->> Internal Processes: Account Booked
    Quantum API ->> Unit: [CWH] Account Created
    
```
