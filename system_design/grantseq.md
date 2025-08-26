## **Grant Sequence Logic:**

**Same User + Same 5-Min Window** = Sequential numbering (-01, -02, -03, etc.)

### **Example Scenario:**
John submits multiple grant requests:

| Time | User | 5-Min Window | GrantID | Sequence |
|------|------|--------------|---------|----------|
| 16:10 | john | 1610 | `DMS-GRANT-20250826-1610-john-01` | **01** (first) |
| 16:12 | john | 1610 | `DMS-GRANT-20250826-1610-john-02` | **02** (second) |
| 16:14 | john | 1610 | `DMS-GRANT-20250826-1610-john-03` | **03** (third) |
| 16:15 | john | 1615 | `DMS-GRANT-20250826-1615-john-01` | **01** (new window) |

### **Different Users in Same Window:**
| Time | User | 5-Min Window | GrantID | 
|------|------|--------------|---------|
| 16:10 | john | 1610 | `DMS-GRANT-20250826-1610-john-01` |
| 16:12 | mary | 1610 | `DMS-GRANT-20250826-1610-mary-01` |
| 16:13 | john | 1610 | `DMS-GRANT-20250826-1610-john-02` |
| 16:14 | mary | 1610 | `DMS-GRANT-20250826-1610-mary-02` |

## **Key Points:**
- ✅ **Per-user sequencing**: Each user has their own sequence count
- ✅ **Per-window reset**: Sequence resets to -01 for each new 5-minute window
- ✅ **Collision prevention**: No duplicate GrantIDs possible
- ✅ **Clear tracking**: Easy to see multiple requests from same user in same time period

This design handles scenarios where:
- Users submit multiple permission requests rapidly
- Multiple users request permissions simultaneously  
- System needs unique identifiers for audit trails
- You want to track user behavior patterns (frequent requesters)

The sequence number essentially answers: *"How many times has this specific user requested grants in this specific 5-minute window?"*
