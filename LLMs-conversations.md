1. Tell me the Azure Cosmos DB storage tiers, it's cost structure.
    Cosmos DB stores all data in high-performance SSD storage

    Cost Structure:
      Provisioned Throughput: Pay for reserved RU/s (Request Units)
      Serverless Mode: Pay per RU consumption
      Storage Cost: Fixed $0.25/GB/month (regardless of access frequency)

2. So, does it not have cold storage tier?
    No built-in cold storage tier. But can use Cosmos DB Cold Tier (Preview): $0.015/GB/month for cold data.

3. Give me an Architecture Diagram, which uses Cosmos DB for hot tier (data less than 3 months) and Azure blob cold storage (for data more than 3 months).
    -- Mentioned in Readme file

4. Provide me the Pseudo code for Archieval and Retrieval process.
    -- Mentioned in Readme file

5. Now, let's look into some Critical Production Risks & Mitigation Strategies.
    Data Loss During Archival
    Scenario: Power outage during archival causes records to be deleted from Cosmos DB but not saved to Blob Storage
    Precaution: Ensure Cosmos DB deletion happens **only after** successful Blob write.
    
    Retrieval Performance Degradation
    Scenario: If archived record requests spike, retrieval may exceed 5s latency.
    Precaution: Add Redis caching for cold data.
    
    Excessive Records Cross Threshold
    Scenario: Excessive (>500K) Records Cross Threshold Simultaneously can cause Function timeouts, latency, etc.
    Precaution: We break the archival into **smaller, manageable chunks** and use **horizontal scaling** to speed up the process without overloading the system.

6. What can we do for backups, like snaphots for Azure blob and?
    Failover to Secondary Region by Enable **multi-region replication** for Cosmos DB.
    Use Cosmos DB Backup for Point-in-Time Restore
