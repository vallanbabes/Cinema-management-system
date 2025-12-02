# Cinema Management System – Use Case Analysis

## Actors

### **Administrator**
The only system user with full access to all management functions including hall management, showtime scheduling, filtering, bulk operations, and deletion.

## Use Case Scenarios

### **UC1: Process for Creating a New Hall**

**Actor:** Administrator  
**Precondition:** Administrator is authenticated in the system.

**Flow of Events:**
1. Administrator sends POST request to `/api/halls` with JSON: `{"name": "Hall A", "capacity": 150}`
2. System validates:
   - Name is not blank and ≤ 10 characters
   - Capacity is between 1 and 300
   - Name is unique (not already used in system)
3. If validation passes, creates hall in database with generated ID
4. Returns 201 Created: `{"id": 1, "name": "Hall A", "capacity": 150}`
5. Hall is now available for showtime scheduling

**Alternative Flows:**
- Invalid capacity → Returns 400: "Capacity must be between 1 and 300"
- Name too long → Returns 400: "Hall name must be ≤ 10 characters"
- Duplicate name → Returns 409: "Hall name already exists"

**Postcondition:** New hall created in system.

---

### **UC2: Process for Adding a Showtime**

**Actor:** Administrator  
**Precondition:** Hall exists in system.

**Flow of Events:**
1. Administrator sends POST to `/api/showtimes/{hallId}` with JSON: `{"filmTitle": "Inception", "dateTime": "2024-01-15T18:30:00"}`
2. System validates:
   - Film title not blank and ≤ 100 characters
   - DateTime is in ISO format and in the future
   - No overlapping showtime in same hall
   - Hall exists
3. Creates showtime in database with ID
4. Adds showtime to cache for performance
5. Returns 201 Created with showtime details

**Alternative Flows:**
- Past date/time → Returns 400: "Showtime must be in future"
- Time conflict → Returns 409: "Hall already has showtime at this time"
- Hall not found → Returns 404: "Hall not found"

**Postcondition:** Showtime scheduled in hall.

---

### **UC3: Process for Getting Hall Information with Showtimes**

**Actor:** Administrator  
**Precondition:** Administrator wants to view hall details.

**Flow of Events:**
1. Administrator sends GET to `/api/halls/{id}`
2. System retrieves hall details from database
3. Retrieves all showtimes for this hall
4. Checks cache for showtime data (performance optimization)
5. Returns 200 OK: `{"id": 1, "name": "Hall A", "capacity": 150, "showtimes": [...]}`

**Alternative Flows:**
- Hall not found → Returns 404: "Hall not found"
- No showtimes → Returns hall with empty showtimes array

**Postcondition:** Administrator receives complete hall information.

---

### **UC4: Process for Filtering Showtimes by Date**

**Actor:** Administrator  
**Precondition:** Administrator wants to view showtimes for specific date.

**Flow of Events:**
1. Administrator sends GET to `/api/showtimes/filter/date/{hallId}?date=2024-01-15`
2. System validates date format (YYYY-MM-DD)
3. Checks cache for this date/hall combination
4. Queries database for showtimes on specified date
5. Returns filtered showtimes list
6. Caches results for future requests

**Alternative Flows:**
- Invalid date → Returns 400: "Invalid date format (use YYYY-MM-DD)"
- No showtimes → Returns empty array

**Postcondition:** Administrator receives showtimes for specific date.

---

### **UC5: Process for Updating Hall Information**

**Actor:** Administrator  
**Precondition:** Hall exists in system.

**Flow of Events:**
1. Administrator sends PUT to `/api/halls/{id}` with updated data
2. System validates:
   - Hall exists
   - New capacity between 1-300
   - If reducing capacity, ensure no existing showtimes exceed new limit
3. Updates hall in database
4. Updates related showtime calculations if capacity changed
5. Returns 200 OK with updated hall

**Alternative Flows:**
- Capacity conflict → Returns 400: "Cannot reduce capacity below existing showtimes"
- Not found → Returns 404: "Hall not found"

**Postcondition:** Hall information updated in system.

---

### **UC6: Process for Bulk Hall Creation**

**Actor:** Administrator  
**Precondition:** Administrator needs to create multiple halls at once.

**Flow of Events:**
1. Administrator sends POST to `/api/halls/bulk` with array of hall objects
2. System starts database transaction
3. Processes each hall:
   - Validates individual hall data
   - Creates hall if valid, skips if invalid
4. Commits transaction if any halls created successfully
5. Returns 201 with created halls and error list for failed ones

**Alternative Flows:**
- All halls invalid → Returns 400 with all validation errors
- Partial success → Returns created halls list with error details

**Postcondition:** Multiple halls created in single operation.

---

### **UC7: Process for Deleting a Showtime**

**Actor:** Administrator  
**Precondition:** Showtime exists in system.

**Flow of Events:**
1. Administrator sends DELETE to `/api/showtimes/{id}`
2. System validates showtime exists
3. Deletes showtime from database
4. Removes showtime from cache
5. Returns 204 No Content

**Alternative Flows:**
- Showtime not found → Returns 404: "Showtime not found"
- Database error → Returns 500: "Internal server error"

**Postcondition:** Showtime removed from system and cache.
