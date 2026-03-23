
## **Module 21: Task Management **

### **Overview**

This module is designed for the **Technician Manager** to plan, assign, and track service tasks for technicians. The system should support both **daily task allocation** and **monthly pre-planning**, primarily based on data from the **Sales Order (SO) module**.

---

## **Core Objectives**

* Allow managers to **plan tasks for an entire month in advance**
* Enable **daily assignment of tasks to technicians**
* Track **task execution, materials used, and performance**
* Ensure **no conflicts in technician allocation**
* Provide a **simple and user-friendly interface**

---

## **Module Dependencies**

This module integrates with the following:

* **SO (Sales Order)** → Main source for task creation
* **Stock Management** → Tracks chemical/material usage after task completion
* **GMA / Customer / Quotation / Contract Modules** → Provide:

  * Service details
  * Duration
  * Required materials
  * Customer information

---

## **Task Planning Logic**

* Tasks are created **service-wise from SO**
* Each task includes:

  * Number of technicians required
  * Assigned technicians
  * Primary technician (main responsible person)
  * Secondary/support technicians

---

## **User Interface Flow**

### **1. Calendar View (Default Screen)**

* Displays tasks in a **date-wise calendar format**
* Features:

  * **Today’s date highlighted**
  * **Pending tasks from previous days shown as alerts**
  * Easy navigation between dates

---

### **2. Daily Task View**

* When a user clicks on a specific date:

  * Opens a **table/grid view**
  * Displays all tasks for that day with:

    * Time slots
    * Assigned technicians
    * Basic task details

* System must support:

  * **One-to-many and many-to-one relationships**
  * **No scheduling conflicts** (same technician cannot be assigned to overlapping tasks)

---

### **3. Task Detail View**

* Clicking on a task opens a **detailed form**
* Includes:

  * Customer details
  * Service details
  * Task duration
  * Materials/chemicals used
  * Task status
  * Customer feedback/review
  * Download/print option

---

## **Task Creation Flow**

### **Add Task**

* User clicks **“Add Task”** opens 2 tabs (From SO/ From Customer Tickets)

* System shows:

  * List of **Sales Orders (SO)**
  * Customer support tasks for the selected date

* User can:

  * Select **single or multiple tasks**
  * Click **Create Task**

---

### **Assignment Step**

* Assign technicians:

  * Select **multiple technicians**
  * Mark:

    * **Primary technician**
    * **Secondary technicians**

---

## **Task Types**

### **1. Normal Task (From SO)**

* Automatically fetches:

  * Service details
  * Materials required
  * Duration
* Fields are mostly **pre-filled and editable if needed**

---

### **2. Re-Task (Customer Complaint / Bad Review)**

* Triggered when customer raises an issue
* Created by customer support team
* Requires:

  * Manual entry of:

    * Required materials
    * Additional task details
* Some data can still be fetched from SO
* Editable as needed

---

## **Stock Management Integration**

* After task completion:

  * **Materials/chemicals used are recorded**
  * Stock is automatically updated

---

## **Tracking & Reporting**

* Track:

  * Task completion status
  * Technician performance
  * Material usage
  * Customer feedback

---

## **Access Control (RBAC)**

* Role-Based Access Control should be applied:

  * Only authorized users can:

    * Create tasks
    * Assign technicians
    * View/edit task details
* Keep it **simple and user-friendly**
* No approval workflow required

---

## **Key Requirements**

* Dynamic and flexible task assignment
* Conflict-free scheduling
* Easy navigation (calendar + table views)
* Seamless integration with existing modules
* Beginner-friendly UI/UX


---so next in future modules will have Customer support Ticket creation and management modules