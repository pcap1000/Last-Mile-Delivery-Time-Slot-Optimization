# Last-Mile-Delivery-Time-Slot-Optimization

## **Introduction**

Efficient delivery routing is essential for companies that handle large-scale deliveries in urban environments. As delivery demands increase, ensuring timely and accurate deliveries while optimizing for limited resources, such as drivers and vehicles, becomes a complex challenge. The goal of this system is to optimize delivery routes for multiple drivers while accommodating user-selected time slots and adhering to strict delivery time constraints.

In this project, we developed an methodology that divides delivery locations among drivers, assigns user-selected time slots, and ensures that each delivery is completed within a set time frame. The system is designed to dynamically adjust delivery routes based on time constraints, geographic location, and the number of drivers, all while optimizing for efficiency and minimizing travel time.

---

### **Complete Process for Delivery Routing and Time Slot Optimization:**

1. **Plot Delivery Locations**:
   - The system starts by **plotting all delivery locations** on a map using their geocode data (latitude and longitude).
   - For instance, if there are 1000 delivery points, these are visualized for easy route planning.

2. **Dividing Delivery Locations Among Drivers**:
   - Based on the number of drivers available, the system **divides the total delivery points** among them equally. For example, with 10 drivers and 1000 points, each driver is assigned 100 locations to cover during their workday.

3. **Time Constraints and Delivery Efficiency**:
   - Each driver has a fixed number of working hours per day (usually 8 hours or 480 minutes).
   - To ensure deliveries are completed within this timeframe, the system calculates the **average number of deliveries per hour** the driver must achieve. In this case, it could be around 12.5–13 deliveries per hour.
   - As a result, each delivery, including travel and drop-off time, needs to be completed in an average of **4.8 minutes**.

4. **User Time Slot Selection**:
   - The delivery day is split into **multiple time slots** (e.g., 8 slots over the day).
   - Users select their preferred time slots for delivery when placing an order.
   - The system records the selected time slot along with the user's delivery location.

5. **Handling Multiple Users Choosing the Same Time Slot**:
   - When a second user selects the same time slot as another user, the system must determine if it is possible to **accommodate both users** within the same slot without violating the average delivery time constraint (4.8 minutes per delivery).
   - The system checks the **feasibility** by calculating the time required to travel between the locations of both users. If the travel time between these locations is less than or equal to the allowed delivery time (4.8 minutes), both users can share the time slot.

6. **Feasibility Check for Travel Time Between Users**:
   - The system uses routing algorithms to calculate the **travel time between consecutive delivery points**.
   - If the travel time meets the constraint, the second user is added to the time slot; otherwise, the system may reject the selection and suggest an alternative slot.

7. **Ensuring Route Optimization**:
   - After the time slots are filled for each driver, the system performs **route optimization** to determine the most efficient order of deliveries within each time slot.
   - The goal of the route optimization is to minimize overall travel time and ensure that the driver can complete all deliveries assigned to a time slot within the given hour.

8. **Handling Special Conditions**:
   - For locations where multiple deliveries are needed (such as apartments or business complexes), the system may allow **more users** to share a time slot since there would be **no travel time** between them.
   - In dense delivery areas, the system can optimize the route further by limiting the number of available time slots to reduce back-and-forth travel.

9. **Adjusting for Traffic or Delays**:
   - The system may also account for **traffic or delays** by adding a buffer to the 4.8-minute constraint, ensuring the driver has enough time even under challenging conditions.

---

### **Step-by-Step Breakdown:**

#### **1. Initial Setup and Route Planning for 10 Drivers:**

- **Assigning Delivery Locations**:
  - 1000 delivery locations are divided equally among 10 drivers. So each driver has to cover 100 delivery locations per day.
  
- **Delivery Time Calculation**:
  - Each driver has **480 minutes** to complete their deliveries, and must deliver to about **12.5–13 locations per hour**.
  - The average time allowed per delivery (including driving and dropping off the package) is **4.8 minutes**.

#### **2. User A Selects a Time Slot:**

- **Time Slot Selection**:
  - User A selects the **9:00–10:00 AM** time slot.
  - User A’s delivery location is recorded in the system, and the **latitude and longitude** of their location are noted.
  - **Example**:
    - User A's location: (latitude: 40.702304, longitude: -74.003004).
  - The system assigns this location to the driver’s route for the 9:00–10:00 AM slot.

#### **3. User B Selects the Same Time Slot (Conflict Resolution):**

- **Time Slot Selection**:
  - User B also selects the **9:00–10:00 AM** time slot.
  - **Feasibility Check**: The system must now check whether it’s feasible for the driver to deliver to both **User A** and **User B** within the same time slot while meeting the 4.8-minute constraint.
  
#### **4. Travel Time Calculation Between User A and User B:**

- **Input Locations**:
  - User B’s location is also recorded in the system. For example:
    - User B's location: (latitude: 40.707688, longitude: -73.982740).
  
- **Calculating Travel Time**:
  - The system uses a routing API (like **OpenRouteService** or **Google Maps API**) to calculate the travel time from User A’s location to User B’s location.
  - The system takes into account factors like **road networks**, **traffic**, and the most **efficient route**.
  
  **Example**:
  - If the travel time from User A’s location to User B’s location is calculated as **3.5 minutes**, this is within the 4.8-minute constraint, so the system allows User B to share the same time slot.

#### **5. Adding User B to the Same Time Slot:**

- Since the travel time between User A and User B is within the 4.8-minute limit, the system adds **User B** to the same **9:00–10:00 AM** time slot.
  
#### **6. Handling More Users Selecting the Same Time Slot:**

- **Scenario**: Let’s say **User C** also selects the same time slot (9:00–10:00 AM).
  
- **Feasibility Check**:
  - The system calculates the travel time between **User B** and **User C**.
  
  **Example**:
  - If the travel time from User B’s location to User C’s location is calculated to be **4.5 minutes**, then User C can also be added to the same time slot.
  
- The process continues for each new user until the travel time constraint is violated, or the driver’s schedule becomes too tight.

#### **7. Handling Travel Time Violations (User Rejection):**

- If the travel time between any two users exceeds the 4.8-minute limit, the system will not allow the new user to share the same time slot.

**Example**:
- **User D** selects the 9:00–10:00 AM time slot.
- The system calculates that the travel time between **User C** and **User D** is **6 minutes** (greater than the 4.8-minute limit).
- In this case, **User D** cannot be added to the same time slot.

#### **8. Suggesting Alternative Time Slots:**

- When a user cannot be added to a time slot because of travel time constraints, the system suggests **alternative time slots** that are still available.
- **Example**:
  - If **User D** cannot be added to the 9:00–10:00 AM time slot, the system might suggest the **10:00–11:00 AM** slot instead.

#### **9. Route Optimization After Time Slot Selection:**

- After a time slot (like **9:00–10:00 AM**) is filled with multiple users (e.g., User A, User B, and User C), the system optimizes the driver’s route for that time slot.
  
**Optimization Process**:
- The system finds the most efficient sequence of deliveries, minimizing travel time between locations.
- **Example**:
  - The driver might visit **User A** first, then **User B**, and finally **User C**, based on proximity and road conditions.
  
- The system ensures that the driver can deliver to all users within the time slot while staying within the overall constraint of **12.5–13 deliveries per hour** and the **4.8-minute travel limit** between consecutive deliveries.

---

### **Special Cases:**

#### **1. Handling Multiple Deliveries to the Same Location:**

- **Apartment Complexes or Business Buildings**:
  - If multiple users have the same address (e.g., an apartment complex), the system might allow more users to share the same time slot because there is **no travel time** between their deliveries.
  - In such cases, the system can assign multiple deliveries at once without violating the 4.8-minute constraint.

#### **2. Handling Dense Areas (High-Demand Streets):**

- In densely populated areas where many deliveries occur in close proximity (e.g., busy streets), the system may allow more users to share a time slot by taking advantage of the **shorter travel times** between nearby locations.
- It can also reduce the number of available time slots in such areas to minimize travel back and forth, ensuring efficient routing.

#### **3. Adjusting for Traffic or Delays:**

- To account for potential traffic or delays, the system can **add buffer time** to the 4.8-minute constraint, making sure the driver has enough time to complete deliveries even under less-than-ideal conditions.

---


### **Summary**:
- The process involves plotting locations, dividing them among drivers, and ensuring that each driver meets the average delivery time constraint of 4.8 minutes per delivery.
- Users choose time slots, and the system allows multiple users in the same time slot only if the travel time between deliveries stays within the required limit.
- The system optimizes the delivery routes for each driver, ensuring that all deliveries in a time slot are completed efficiently and within the designated time.
