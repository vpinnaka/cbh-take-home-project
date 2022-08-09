# Ticket Breakdown
We are a staffing company whose primary purpose is to book Agents at Shifts posted by Facilities on our platform. We're working on a new feature which will generate reports for our client Facilities containing info on how many hours each Agent worked in a given quarter by summing up every Shift they worked. Currently, this is how the process works:

- Data is saved in the database in the Facilities, Agents, and Shifts tables
- A function `getShiftsByFacility` is called with the Facility's id, returning all Shifts worked that quarter, including some metadata about the Agent assigned to each
- A function `generateReport` is then called with the list of Shifts. It converts them into a PDF which can be submitted by the Facility for compliance.

## You've been asked to work on a ticket. It reads:

**Currently, the id of each Agent on the reports we generate is their internal database id. We'd like to add the ability for Facilities to save their own custom ids for each Agent they work with and use that id when generating reports for them.**


Based on the information given, break this ticket down into 2-5 individual tickets to perform. Provide as much detail for each ticket as you can, including acceptance criteria, time/effort estimates, and implementation details. Feel free to make informed guesses about any unknown details - you can't guess "wrong".


You will be graded on the level of detail in each ticket, the clarity of the execution plan within and between tickets, and the intelligibility of your language. You don't need to be a native English speaker, but please proof-read your work.

## Your Breakdown Here

**Ticket 1:**

Create a database table called `FacilityAgent` with the following columns:
- `id` - primary key
- `facility_id` - foreign key to the `Facility` table
- `agent_id` - foreign key to the `Agent` table
- `custom_id` -  the custom id of the Agent for the Facility (string)
- `created_at` - the date the record was created
- `updated_at` - the date the record was last updated

Implementation Details:
- Add the table schema under `db/schema.prisma` with primary key and foreign key constraints
- Use `npx run db_migrate` to create a new migration file


Acceptance Criteria:
- The new database model should be created with the above columns and the foreign keys set to cascade delete
- Add a new migration file called `create_facility_agent_table` to create the table
- Add unit tests for the new table model to verify that it is created correctly and that the foreign keys are set to cascade delete
- Add unit tests for the new table model to verify inserting and updating a record works correctly


Story Points: 1


**Ticket 2:**

Create a function `postFacilityAgentCustomId` that creates a new record with  `custom_id` field in `FacilityAgent` table. The function should accept the following parameters:
- `facility_id` - the id of the Facility the Agent is working for
- `agent_id` - the id of the Agent
- `custom_id` - the custom id of the Agent for the Facility

Implementation Details:
- Add the function to `facility_agent.controller.js` and implement it
- The function should return the id of the new record created in the `FacilityAgent` table if successful
- The function should throw an exception if unsuccessful and propagate the error message
- The function should throw an exception if the `custom_id` is already in use for the given `facility_id`
- The function should throw an exception if the `facility_id` or `agent_id` are not found in the `Facility` or `Agent` tables
- The function should throw an exception if the `custom_id` is not provided
- The function should throw an exception if the `custom_id` is not a string

Acceptance Criteria:
- The function should create a new record in the `FacilityAgent` table with the given parameters
- Add unit tests for the function to verify that it creates a new record in the `FacilityAgent` table if successful
- Add unit tests for the function to verify that it throws an exception if the `custom_id` is already in use for the given `facility_id`
- Add unit tests for the function to verify that it throws an exception if the `facility_id` or `agent_id` are not found in the `Facility` or `Agent` tables
- Add unit tests for the function to verify that it throws an exception if the `custom_id` is not provided
- Add unit tests for the function to verify that it throws an exception if the `custom_id` is not a string

Story Points: 1


**Ticket 3:**

Create a function `getFacilityAgentCustomId` that returns the custom id of the Agent for the Facility. The function should accept the following parameters:
- `facility_id` - the id of the Facility the Agent is working for
- `agent_id` - the id of the Agent in the database

Implementation Details:
- Add the function to `facility_agent.controller.js` and implement it   
- The function should return the custom id of the Agent for the Facility if successful
- The function should return null if the Agent is not found in the `FacilityAgent` table

Acceptance Criteria:
- Add unit tests for the function to verify that it returns the custom id of the Agent for the Facility if successful
- Add unit tests for the function to verify that it returns null if the Agent is not found in the `FacilityAgent` table

Story Points: 1

**Ticket 4:**

Use `custom_id` from the `FacilityAgent` table to while generating reports for the Facilities.

Implementation Details:
- Update the `generateReport` function to use the `getFacilityAgentCustomId` function to get the custom id of the Agent for the Facility
- If the custom id is not found, the `generateReport` function should fall back to the original database Agent id

Acceptance Criteria:
- Add unit tests for the function to verify that it uses the `getFacilityAgentCustomId` function to get the custom id of the Agent for the Facility
- Add unit tests for the function to verify that it falls back to the original database Agent id if the custom id is not found

Story Points: 1






