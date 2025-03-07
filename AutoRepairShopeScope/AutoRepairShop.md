### Conceptual EER Diagram for Auto Repair Shop

#### **Entities and Attributes**
1. **Customer (Cliente)**
   - customer_id (PK)
   - name
   - address
   - phone

2. **Vehicle (Veículo)**
   - vehicle_id (PK)
   - license_plate (Unique)
   - model_description
   - owner_id (FK -> Customer)

3. **Mechanic (Mecânico)**
   - mechanic_id (PK)
   - name
   - address
   - specialty

4. **Team (Equipe de mecânicos)**
   - team_id (PK)
   - mechanic_id (FK)

5. **Service Order (Ordem de Serviço)**
   - order_id (PK)
   - emission_date
   - estimated_completion_date
   - total_value (Services + Parts)
   - status
   - customer_authorization (Boolean)
   - vehicle_id (FK -> Vehicle)
   - team_id (FK -> Team)

6. **Service (Serviço)**
   - service_id (PK)
   - description
   - labor_cost (Determined from Labor Table)

7. **Labor Reference (Tabela de Referência de Mão de Obra)**
   - labor_id (PK)
   - service_type
   - cost

8. **Part (Peça)**
   - part_id (PK)
   - name
   - cost

#### **Relationships and Cardinality**
1. **Customer - Vehicle** (1:N)
   - One customer can own multiple vehicles, but each vehicle belongs to only one customer.

2. **Vehicle - Service Order** (1:N)
   - One vehicle can have multiple service orders, but each service order is for one vehicle.

3. **Mechanic - Team** (N:1)
   - A team is composed of one or more mechanics, but each mechanic belongs to only one team.

4. **Team - Service Order** (1:N)
   - One team handles multiple service orders, but each service order is assigned to only one team.

5. **Service Order - Service** (N:M) (via Order_Service)
   - A service order can include multiple services, and a service can be in multiple service orders.

6. **Service - Labor Reference** (1:1)
   - Each service is associated with a specific labor cost in the labor reference table.

7. **Service Order - Part** (N:M) (via Order_Part)
   - A service order can include multiple parts, and a part can be used in multiple service orders.

#### **Associative Tables for Many-to-Many (N:M) Relationships**
1. **Order_Service** (Intermediate table for Service Order and Service)
   - order_id (FK -> Service Order)
   - service_id (FK -> Service)
   - quantity
   - total_cost

2. **Order_Part** (Intermediate table for Service Order and Part)
   - order_id (FK -> Service Order)
   - part_id (FK -> Part)
   - quantity
   - total_cost

#### **Additional Considerations**
- The **total_value** in Service Order is calculated as the sum of the costs from Order_Service and Order_Part.
- A **service order** cannot be executed without **customer authorization**.
- The **team** assigned to a service order is responsible for both **evaluating and executing** the services.

