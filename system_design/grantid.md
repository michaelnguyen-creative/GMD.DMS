# Grant ID Design Specifications

## Overview
The Grant ID system groups permission requests into logical batches based on temporal proximity, enabling efficient processing while maintaining traceability and supporting complex relationship patterns.

## Core Requirements Analysis

### Relationship Patterns Supported
- **One-to-Many**: Single person receives multiple grants within same time window
- **Many-to-One**: Multiple people receive grants for same resource within time window  
- **Many-to-Many**: Complex scenarios where multiple people get multiple grants

### Key Design Principles
1. **Temporal Grouping**: Requests within same time window share Grant ID
2. **Deterministic Generation**: Same inputs always produce same Grant ID
3. **Human Readable**: Format allows easy identification and sorting
4. **Collision Resistant**: Minimal chance of unintended grouping
5. **Audit Friendly**: Clear traceability for compliance requirements

## Grant ID Generation Logic

### Base Format
```
YYYYMMDD-HHmm
```

**Components:**
- `YYYY`: 4-digit year
- `MM`: 2-digit month (01-12)
- `DD`: 2-digit day (01-31)
- `HH`: 2-digit hour in 24-hour format (00-23)
- `mm`: 2-digit minute rounded to nearest interval

### Scoped Format
```
<SYSTEM>-<ACTION>-YYYYMMDD-HHmm-<USER>-<SEQ>
```

**Additional Components:**
- `<SYSTEM>`: System identifier (i.e., `DMS`)
- `<ACTION>: Action identifer (i.e., `GRANT`)
- `<USER>`: User identifer (i.e., email = "abc@company.com", USER = "ABC")
- `<SEQ>`: Grant identier in the same time window (GrantId)

**Examples:**
- `DMS-GRANT-20250826-1420-ABC-01`

### Time Window Determination

```
30-MINUTE WINDOW LOGIC:
- Rounds time down to nearest 30-minute interval
- Examples: 16:07->1600, 16:10->1600, 16:32->1630, 16:15->1615
- Prevents collision for requests within same 5-minute window
- Each user gets sequence number within their window
```

## **30-Minute Window Examples:**

| Real Time | Minutes | Calculation | 30-Min Window | Result |
|-----------|---------|-------------|---------------|---------|
| 14:07 | 07 | 07÷30=0 → 0×30=00 | 1400 | `DMS-GRANT-20250826-1400-john-01` |
| 14:15 | 15 | 15÷30=0 → 0×30=00 | 1400 | `DMS-GRANT-20250826-1400-mary-01` |
| 14:29 | 29 | 29÷30=0 → 0×30=00 | 1400 | `DMS-GRANT-20250826-1400-admin-01` |
| 14:30 | 30 | 30÷30=1 → 1×30=30 | 1430 | `DMS-GRANT-20250826-1430-john-01` |
| 14:45 | 45 | 45÷30=1 → 1×30=30 | 1430 | `DMS-GRANT-20250826-1430-mary-01` |
| 14:59 | 59 | 59÷30=1 → 1×30=30 | 1430 | `DMS-GRANT-20250826-1430-admin-01` |
| 15:00 | 00 | 00÷30=0 → 0×30=00 | 1500 | `DMS-GRANT-20250826-1500-john-01` |

```
SEQUENCE NUMBERING:
- Filters existing GrantIDs matching same date+window+user
- Adds 1 to count for next sequence number
- Uses padLeft for consistent 2-digit format (01, 02, etc.)
```

#### Option 1: Fixed Interval Rounding (Recommended/Chosen)
```
Interval: 5 minutes
Windows: 00, 05, 10, 15, 20, 25, 30, 35, 40, 45, 50, 55

Logic:
- Round DOWN to nearest 5-minute mark
- Example: 14:23 → 14:20, 14:27 → 14:25
```

#### Option 2: Sliding Window (Alternative)
```
Window Size: 5 minutes
Logic: Requests within 5 minutes of first request get same Grant ID

Pros: More flexible grouping
Cons: Complex state management, potential infinite extension
```

#### Option 3: Hybrid Approach (Advanced)
```
Primary: Fixed intervals (Option 1)
Secondary: Extension logic for active batches

Rules:
- If new request arrives within 2 minutes of last request in active batch
- AND within 10 minutes of batch start
- THEN extend existing Grant ID
```

## Detailed Technical Specifications

### Time Zone Handling
```
Standard: UTC for consistency
Local Time ("Asia Standard SE") Consideration: Convert to UTC before processing

Decision Rationale:
- Eliminates DST complications
- Ensures global consistency
- Simplifies cross-timezone operations
```

### Interval Size Analysis

| Interval | Pros | Cons | Use Cases |
|----------|------|------|-----------|
| 1 minute | High precision | Too granular, many small batches | Real-time critical systems |
| 5 minutes | Balanced grouping | Good for most scenarios | **Recommended default** |
| 15 minutes | Larger batches | May group unrelated requests | Bulk processing scenarios |
| 30 minutes | Very large batches | Poor granularity | Legacy system migration |


**Recommendation: 5 minutes**
- Optimal balance between grouping efficiency and precision
- Aligns with typical user behavior patterns
- Manageable batch sizes for processing

### Edge Cases & Handling

#### Midnight Boundary
```
Scenario: Request at 23:59, another at 00:01 next day
Result: Different Grant IDs (different dates)
Rationale: Daily boundaries maintain audit clarity
```

#### Month/Year Boundaries
```
Similar logic: Natural calendar boundaries preserved
Benefits: Simplified reporting and archival
```

#### System Clock Synchronization
```
Issue: Servers with different times
Mitigation: Use centralized timestamp source (SharePoint system time)
Fallback: Client-provided timestamps with validation
```

### Grant ID Uniqueness Guarantees

#### Collision Probability
```
Format: YYYYMMDD-HHmm
Theoretical collisions: Only within same 5-minute window
Practical collisions: Expected and desired (grouping mechanism)
```

#### Uniqueness Scope
```
Per Environment: Unique within SharePoint tenant/site
Cross-Environment: Prefix with environment/system identifier if needed
Format: [ENV/SYS-]YYYYMMDD-HHmm
Example: PROD-20250826-1425
```

## Advanced Design Decisions

### State Management Options

#### Option A: Stateless (Recommended/Chosen)
```
Approach: Generate Grant ID based purely on timestamp
Benefits: 
- Simple implementation
- No state storage required  
- Highly reliable
- Easy to reproduce/debug

Drawbacks:
- Less flexible grouping
- Cannot handle complex business rules
```

## Implementation Variants

### Variant 1: Simple Time-Based (Default)
```
Grant ID: YYYYMMDD-HHmm (5-minute intervals)
Use Case: Standard permission requests
Complexity: Low
Maintenance: Minimal
```

### Variant 2: Enhanced with Sequence
```
Grant ID: YYYYMMDD-HHmm-SS (with sequence counter)
Use Case: High-volume environments
Complexity: Medium
Benefits: Better batch tracking
```

### Variant 3: Business Context Aware
```
Grant ID: YYYYMMDD-HHmm-[CONTEXT]
Context examples:
- BULK: Bulk operations
- URGENT: Priority requests  
- DEPT-[CODE]: Department-specific batches

Use Case: Enterprise environments with complex workflows
Complexity: High
Benefits: Rich categorization and reporting
```

### Variant 4: Hybrid Temporal-Logical
```
Primary: Time-based grouping
Secondary: Logical relationship analysis

Example:
- Same requester + same target folder = extend window to 15 minutes
- Same recipient + multiple folders = group regardless of time
- Emergency requests = immediate processing (unique Grant ID)
```

## Performance Considerations

### Generation Speed
```
Target: < 100ms per Grant ID generation
Factors affecting performance:
- Timestamp formatting operations
- String concatenation
- Validation logic complexity
```

### Batch Processing Efficiency
```
Optimal batch sizes: 10-100 requests per Grant ID
Performance monitoring:
- Track average batch size
- Monitor processing time per batch
- Alert on oversized batches
```

### Database Impact
```
Indexing strategy:
- Primary index on Grant ID
- Secondary index on Created Date
- Composite index on (Grant ID, Status)

Query optimization:
- Batch retrieval by Grant ID
- Time-range queries for reporting
- Status-based filtering
```

## Monitoring & Observability

### Key Metrics
```
- Grant ID generation rate
- Average batch size per Grant ID
- Time distribution of requests
- Grant ID collision analysis (intentional grouping verification)
- Processing time per Grant ID batch
```

### Alerting Thresholds
```
- Batch size > 100 requests: Investigation needed
- Grant ID generation time > 500ms: Performance issue
- No requests for 1 hour during business hours: System health check
- Failed Grant ID generation rate > 5%: Critical alert
```

### Audit Trail Requirements
```
Tracked events:
- Grant ID generation with source timestamp
- Batch formation logic decisions
- Any manual Grant ID overrides
- Grant ID lifecycle (created → processing → completed)
```

## Testing Strategy

### Integration Tests
```
Scenarios:
- Multiple requests in same time window
- Requests spanning midnight boundary  
- High-volume batch processing
- Error recovery and retry logic
- Cross-timezone request handling
```

### Load Testing
```
Parameters:
- Concurrent Grant ID generation: 100+ requests/second
- Batch processing with 1000+ items per Grant ID
- Extended operation over 24-hour periods
- Memory usage under sustained load
```

## Migration & Rollback Strategy

### Data Migration
```
From existing system:
1. Export current permission requests
2. Generate Grant IDs for historical data
3. Validate data integrity
4. Cutover to new system

Grant ID backfill logic:
- Use original request timestamps
- Apply same generation rules retroactively
- Maintain audit trail of migration
```

### Rollback Plan
```
Rollback triggers:
- Grant ID generation failure rate > 10%
- Performance degradation > 50%
- Data integrity issues detected

Rollback process:
1. Switch to backup Grant ID generation
2. Queue requests for later processing
3. Investigate and fix issues
4. Resume normal operation with data reconciliation
```

### Runtime Configuration
```
Adjustable parameters:
- Time interval size (1, 5, 15, 30 minutes)
- Grant ID format template
- Batch size thresholds
- Processing timeouts
- Error retry policies
```
