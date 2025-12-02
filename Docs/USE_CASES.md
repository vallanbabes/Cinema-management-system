# Cinema Management System – Use Case Analysis

## Actors

### **Cinema Manager**
An employee responsible for managing cinema halls and showtimes. Has permissions for basic CRUD operations.

### **Administrator**
System administrator with full access including bulk operations and advanced functions.

### **Customer (API Consumer)**
External system or user accessing the API to view showtimes and hall information.

## Use Case Scenarios

### **UC1: Process for Creating a New Hall**

**Actor:** Cinema Manager  
**Precondition:** Manager is authenticated and has create hall permission.

**Flow of Events:**
1. Manager sends POST request to `/api/halls` with JSON containing name and capacity
2. System validates:
   - Name is not blank and ≤ 10 characters
   - Capacity is between 1 and 300
   - Name is unique in the system
3. If validation passes, system creates hall in database
4. Returns 201 Created with hall details including ID
5. Hall is now available for showtime scheduling

**Alternative Flows:**
- Invalid capacity → Returns 400: "Capacity must be between 1 and 300"
- Name too long → Returns 400: "Hall name must be ≤ 10 characters"
- Duplicate name → Returns 409: "Hall name already exists"

**Postcondition:** New hall created and ready for use.

---

### **UC2: Process for Adding a Showtime**

**Actor:** Cinema Manager  
**Precondition:** Hall exists and manager has schedule permission.

**Flow of Events:**
1. Manager sends POST to `/api/showtimes/{hallId}` with filmTitle and dateTime
2. System validates:
   - Film title not blank and ≤ 100 characters
   - DateTime is in ISO format and in the future
   - No showtime conflict in same hall
   - Hall exists and is active
3. Creates showtime in database
4. Caches showtime for performance
5. Returns 201 Created with showtime details

**Alternative Flows:**
- Past date/time → Returns 400: "Showtime must be in future"
- Time conflict → Returns 409: "Hall occupied at this time"
- Hall not found → Returns 404: "Hall not found"

**Postcondition:** Showtime scheduled and cached.

---

### **UC3: Process for Getting Hall Information with Showtimes**

**Actor:** Customer  
**Precondition:** Customer knows hall ID.

**Flow of Events:**
1. Customer sends GET to `/api/halls/{id}`
2. System retrieves hall details from database
3. System retrieves all showtimes for this hall
4. Checks cache for showtime data
5. Formats response with hall info and showtimes list
6. Returns 200 OK with complete hall information

**Alternative Flows:**
- Hall not found → Returns 404: "Hall not found"
- No showtimes → Returns hall info with empty showtimes array
- Cache hit → Returns cached data faster

**Postcondition:** Customer receives complete hall information.

---

### **UC4: Process for Filtering Showtimes by Date**

**Actor:** Customer  
**Precondition:** Customer knows hall ID and date.

**Flow of Events:**
1. Customer sends GET to `/api/showtimes/filter/date/{hallId}?date=YYYY-MM-DD`
2. System validates date format
3. Checks cache for this date/hall combination
4. If not cached, queries database for showtimes on specified date
5. Returns filtered showtimes list
6. Caches results for future requests

**Alternative Flows:**
- Invalid date → Returns 400: "Invalid date format"
- No showtimes → Returns empty array
- Cache hit → Returns results faster

**Postcondition:** Customer receives showtimes for specific date.

---

### **UC5: Process for Updating Hall Information**

**Actor:** Cinema Manager  
**Precondition:** Hall exists and manager has update permission.

**Flow of Events:**
1. Manager sends PUT to `/api/halls/{id}` with updated data
2. System validates:
   - Hall exists
   - New capacity between 1-300
   - If reducing capacity, ensure no showtimes exceed new limit
3. Updates hall in database
4. Updates related showtime calculations if capacity changed
5. Returns 200 OK with updated hall

**Alternative Flows:**
- Capacity conflict → Returns 400: "Cannot reduce below existing bookings"
- Not found → Returns 404: "Hall not found"
- Invalid data → Returns 400 with validation errors

**Postcondition:** Hall information updated.

---

### **UC6: Process for Bulk Hall Creation**

**Actor:** Administrator  
**Precondition:** Admin has bulk operation permission.

**Flow of Events:**
1. Admin sends POST to `/api/halls/bulk` with array of hall objects
2. System starts database transaction
3. Processes each hall sequentially:
   - Validates individual hall
   - Creates if valid, skips if invalid
4. Commits transaction if any halls created
5. Returns 201 with created halls and error list

**Alternative Flows:**
- All invalid → Returns 400 with all errors
- Partial success → Returns created halls and errors
- Transaction fails → Returns 500 with rollback

**Postcondition:** Multiple halls created atomically.

---

### **UC7: Process for Deleting a Showtime**

**Actor:** Cinema Manager  
**Precondition:** Showtime exists and manager has delete permission.

**Flow of Events:**
1. Manager sends DELETE to `/api/showtimes/{id}`
2. System validates:
   - Showtime exists
   - No bookings exist for this showtime
   - Showtime is in future (or admin override)
3. Deletes showtime from database
4. Invalidates cache entry for this showtime
5. Returns 204 No Content

**Alternative Flows:**
- Bookings exist → Returns 400: "Cannot delete showtime with bookings"
- Not found → Returns 404: "Showtime not found"
- Past showtime → Requires admin approval

**Postcondition:** Showtime removed and cache cleared.
