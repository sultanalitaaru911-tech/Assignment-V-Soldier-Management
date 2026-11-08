

## File: sms_202510215.sql

-- Student Number: 202510215
-- Personal values: P = 5, U = 4, M = 1
-- Other collaberaters:202510491
-- Other collaberaters:202510433

--SELECT DATABASE(), CURRENT_USER(), NOW();

-- Creates ranks table
CREATE TABLE ranks (
    rank_id INT PRIMARY KEY,
    rank_name VARCHAR(40) NOT NULL UNIQUE,
    rank_level INT NOT NULL UNIQUE CHECK (rank_level BETWEEN 1 AND 6)
);

-- Creates the units table
CREATE TABLE units (
    unit_id INT PRIMARY KEY,
    unit_name VARCHAR(60) NOT NULL UNIQUE,
    location VARCHAR(80) NOT NULL
);

-- Creates the missions table
CREATE TABLE IF NOT EXISTS missions (
    mission_id INT PRIMARY KEY,
    mission_name VARCHAR(100) NOT NULL UNIQUE,
    start_date DATE NOT NULL,
    end_date DATE NULL,
    CHECK (end_date IS NULL OR end_date >= start_date)
);

-- Creates the soldiers table
CREATE TABLE soldiers (
    soldier_id INT GENERATED ALWAYS AS IDENTITY PRIMARY KEY,
    service_number VARCHAR(40) NOT NULL UNIQUE,
    first_name VARCHAR(50) NOT NULL,
    last_name VARCHAR(50) NOT NULL,
    date_of_birth DATE NOT NULL,
    enlistment_date DATE NOT NULL,
    status VARCHAR(20) NOT NULL DEFAULT 'Active'
	CHECK (status IN('Active','ON leave','retired')),
    rank_id INT NOT NULL,
    unit_id INT NOT NULL,
    CONSTRAINT chk_enlistment_after_birth CHECK (enlistment_date > date_of_birth),
    CONSTRAINT fk_soldiers_rank FOREIGN KEY (rank_id) REFERENCES ranks(rank_id),
    CONSTRAINT fk_soldiers_unit FOREIGN KEY (unit_id) REFERENCES units(unit_id)
);

-- Creates the soldier missions table
CREATE TABLE soldier_missions (
    soldier_id INT NOT NULL,
    mission_id INT NOT NULL,
    mission_role VARCHAR(80) NOT NULL,
    PRIMARY KEY (soldier_id, mission_id),
    CONSTRAINT fk_sm_soldier FOREIGN KEY (soldier_id) REFERENCES soldiers(soldier_id),
    CONSTRAINT fk_sm_mission FOREIGN KEY (mission_id) REFERENCES missions(mission_id)
);

-- Inserts data into the tables
INSERT INTO ranks (rank_id, rank_name, rank_level)
VALUES
(1, 'Private', 1),
(2, 'Corporal', 2),
(3, 'Sergeant', 3),
(4, 'Lieutenant', 4),
(5, 'Captain', 5),
(6, 'Major', 6);

INSERT INTO units (unit_id, unit_name, location)
VALUES
(1, 'Alpha Company', 'North Camp'),
(2, 'Bravo Company', 'South Camp'),
(3, 'Charlie Company', 'East Camp'),
(4, 'Delta Company', 'West Camp'),
(5, 'Signals Unit', 'Central Camp'),
(6, 'Medical Unit', 'Central Camp');

INSERT INTO missions (mission_id, mission_name, start_date, end_date)
VALUES
(1, 'Exercise Falcon', '2025-02-03', '2025-02-28'),
(2, 'Flood Relief', '2025-04-14', '2025-05-30'),
(3, 'Border Patrol', '2025-07-01', NULL),
(4, 'Peace Support', '2025-09-15', NULL),
(5, 'Field Medical Camp', '2026-01-12', '2026-01-23');

--Adds 30 fictional soldiers and
INSERT INTO soldiers(service_number, first_name, last_name, date_of_birth, enlistment_date, status, rank_id, unit_id)
VALUES
('ST-SEED-0001', 'Eric', 'Mugisha', '1998-01-12', '2018-02-01', 'Active', 1, 1),
('ST-SEED-0002', 'Grace', 'Uwimana', '1997-04-23', '2017-05-10', 'Active', 2, 2),
('ST-SEED-0003', 'Jean', 'Habimana', '1995-06-14', '2015-07-01', 'Active', 3, 3),
('ST-SEED-0004', 'Aisha', 'Mukamana', '1999-09-08', '2019-10-01', 'Active', 1, 4),
('ST-SEED-0005', 'Peter', 'Ndayisaba', '1994-11-30', '2014-12-15', 'Active', 4, 5),
('ST-SEED-0006', 'Diane', 'Ingabire', '1996-03-17', '2016-04-01', 'Active', 2, 6),
('ST-SEED-0007', 'Samuel', 'Nkurunziza', '1993-07-21', '2013-08-01', 'Active', 5, 1),
('ST-SEED-0008', 'Alice', 'Nyirahabimana', '2000-02-09', '2020-03-01', 'Active', 1, 2),
('ST-SEED-0009', 'David', 'Nsengiyumva', '1992-12-05', '2012-12-20', 'Active', 6, 3),
('ST-SEED-0010', 'Olive', 'Uwamahoro', '1998-08-18', '2018-09-01', 'Active', 2, 4),
('ST-SEED-0011', 'Patrick', 'Bizimana', '1995-01-25', '2015-02-10', 'Active', 3, 5),
('ST-SEED-0012', 'Mireille', 'Mukeshimana', '1997-05-19', '2017-06-01', 'Active', 1, 6),
('ST-SEED-0013', 'Claude', 'Rukundo', '1991-10-11', '2011-11-01', 'Active', 5, 1),
('ST-SEED-0014', 'Beata', 'Nyinawumuntu', '1999-06-02', '2019-07-01', 'Active', 2, 2),
('ST-SEED-0015', 'Emmanuel', 'Karangwa', '1996-04-14', '2016-05-01', 'Active', 4, 3),
('ST-SEED-0016', 'Solange', 'Umutoni', '2000-12-22', '2021-01-15', 'Active', 1, 4),
('ST-SEED-0017', 'Robert', 'Hakizimana', '1994-02-27', '2014-03-15', 'Active', 3, 5),
('ST-SEED-0018', 'Chantal', 'Mukandori', '1998-07-03', '2018-08-01', 'Active', 2, 6),
('ST-SEED-0019', 'Alex', 'Twagirimana', '1990-09-16', '2010-10-01', 'Active', 6, 1),
('ST-SEED-0020', 'Fiona', 'Mutesi', '1997-11-08', '2017-12-01', 'Active', 2, 2),
('ST-SEED-0021', 'Oscar', 'Niyonzima', '1993-03-29', '2013-04-15', 'Active', 4, 3),
('ST-SEED-0022', 'Lydia', 'Nyirahirwa', '1999-01-06', '2019-02-01', 'Active', 1, 4),
('ST-SEED-0023', 'Benoit', 'Munyaneza', '1995-08-24', '2015-09-10', 'Active', 3, 5),
('ST-SEED-0024', 'Esther', 'Karemera', '1996-10-13', '2016-11-01', 'Active', 2, 6),
('ST-SEED-0025', 'Vincent', 'Rugamba', '1992-05-31', '2012-06-15', 'Active', 5, 1),
('ST-SEED-0026', 'Nadia', 'Iradukunda', '2001-04-02', '2021-05-01', 'Active', 1, 2),
('ST-SEED-0027', 'Felix', 'Gatera', '1994-12-19', '2014-01-10', 'Active', 3, 3),
('ST-SEED-0028', 'Jeanne', 'Nyiransabimana', '1998-02-15', '2018-03-01', 'Active', 2, 4),
('ST-SEED-0029', 'Michael', 'Rwamucyo', '1991-07-07', '2011-08-01', 'Active', 6, 5),
('ST-SEED-0030', 'Clarisse', 'Mukiza', '2000-09-26', '2020-10-15', 'Active', 1, 6);

--Adds 30 fictional mission assignments
INSERT INTO soldier_missions (soldier_id, mission_id, mission_role)
VALUES
(1, 1, 'Patrol Member'),
(2, 1, 'Communications'),
(3, 2, 'Relief Coordinator'),
(4, 3, 'Patrol Member'),
(5, 4, 'Team Leader'),
(6, 2, 'Medical Support'),
(7, 3, 'Patrol Supervisor'),
(8, 1, 'Logistics'),
(9, 4, 'Advisor'),
(10, 5, 'Medical Support'),
(11, 2, 'Supply Officer'),
(12, 5, 'Nurse Assistant'),
(13, 3, 'Operations Lead'),
(14, 4, 'Communications'),
(15, 1, 'Section Leader'),
(16, 2, 'Relief Support'),
(17, 3, 'Patrol Member'),
(18, 5, 'Medical Support'),
(19, 4, 'Security Lead'),
(20, 1, 'Logistics'),
(21, 2, 'Transport'),
(22, 3, 'Patrol Member'),
(23, 4, 'Advisor'),
(24, 5, 'Medical Support'),
(25, 1, 'Operations Lead'),
(26, 2, 'Relief Support'),
(27, 3, 'Patrol Member'),
(29, 4, 'Communications'),
(30, 5, 'Medical Support');
-- Soldier 28 intentionally has no assignment, so query C9 returns at least one result.

INSERT INTO soldiers
(soldier_id, service_number, first_name, last_name, date_of_birth, enlistment_date, status, rank_id, unit_id)
VALUES
(31, 'ST-202510215', 'Ali', 'Salaheldeen', '2002-05-14', '2022-06-01', 'Active', 1, 4),
(32, 'ST-202510215-2', 'Daniel', 'Kamanzi', '2001-09-20', '2021-10-15', 'Active', 6, 4),
(33, 'ST-202510215-3', 'Aline', 'Niyonsenga', '2003-02-11', '2023-03-01', 'Active', 2, 5);

-- Assigns soldiers 31 and 32 to mission M = 1.
INSERT INTO soldier_missions (soldier_id, mission_id, mission_role)
VALUES
(31, 1, 'Communications Support'),
(32, 1, 'Medical Coordinator');

-- Shows the most recent 10 newsest soldeiers added
SELECT * FROM soldiers ORDER BY soldier_id DESC LIMIT 10;

-- Shows the most recent 10 newest sodires in ascending order
SELECT * FROM (
    SELECT * FROM soldiers ORDER BY soldier_id DESC LIMIT 10
) AS newest_soldiers
ORDER BY soldier_id ASC;

-- Shows soldiers whose id is 10, 15, or 20
SELECT * FROM soldiers WHERE soldier_id IN (10, 15, 20);

-- Shows IDs 10+P, 15+P and 20+P: 15, 20 and 25. P=5
SELECT * FROM soldiers WHERE soldier_id IN (10 + 5, 15 + 5, 20 + 5);

-- 	Shows soldiers who are active between IDs 10 and 20
SELECT * FROM soldiers
WHERE status = 'Active' AND soldier_id BETWEEN 10 AND 20;

-- Shows each soldier's ID and name with rank name, highest rank level first, then last name
SELECT s.soldier_id, s.first_name, s.last_name, r.rank_name
FROM soldiers AS s
JOIN ranks AS r ON s.rank_id = r.rank_id
ORDER BY r.rank_level DESC, s.last_name ASC;

-- Shows Active soldiers in unit U = 4 with first/last name, rank, unit name and location
SELECT s.first_name, s.last_name, r.rank_name, u.unit_name, u.location
FROM soldiers AS s
JOIN ranks AS r ON s.rank_id = r.rank_id
JOIN units AS u ON s.unit_id = u.unit_id
WHERE s.status = 'Active' AND s.unit_id = 4;

-- Shows everyone assigned to mission M = 1, including mission, soldier, role and unit
SELECT m.mission_name,
       CONCAT(s.first_name, ' ', s.last_name) AS soldier_name,
       sm.mission_role,
       u.unit_name
FROM soldier_missions AS sm
JOIN missions AS m ON sm.mission_id = m.mission_id
JOIN soldiers AS s ON sm.soldier_id = s.soldier_id
JOIN units AS u ON s.unit_id = u.unit_id
WHERE m.mission_id = 1;

--Shows soldiers who have never been assigned to a mission
SELECT s.*
FROM soldiers AS s
LEFT JOIN soldier_missions AS sm ON s.soldier_id = sm.soldier_id
WHERE sm.soldier_id IS NULL;

-- Shows every unit and its soldier count, largest unit first. Units with no soldiers remain visible
SELECT u.unit_name, COUNT(s.soldier_id) AS number_of_soldiers
FROM units AS u
LEFT JOIN soldiers AS s ON u.unit_id = s.unit_id
GROUP BY u.unit_id, u.unit_name
ORDER BY number_of_soldiers DESC, u.unit_name ASC;

-- Examine the query plan before adding an index.
EXPLAIN SELECT * FROM soldiers WHERE last_name = 'Mugisha';

-- Creates the requested index on last_name, lookups filtering by surname
CREATE INDEX idx_soldiers_last_name ON soldiers(last_name);

-- Run the same EXPLAIN again to compare type and key with the first plan
EXPLAIN SELECT * FROM soldiers WHERE last_name = 'Mugisha';

-- Creates the requested view, joining rank and unit details and combining first/last name
CREATE OR REPLACE VIEW v_soldier_profile AS
SELECT s.soldier_id,
       CONCAT(s.first_name, ' ', s.last_name) AS full_name,
       r.rank_name,
       u.unit_name,
       u.location,
       s.status
FROM soldiers AS s
JOIN ranks AS r ON s.rank_id = r.rank_id
JOIN units AS u ON s.unit_id = u.unit_id;

-- Uses the view to show every soldier in unit U = 4, ordered by full name.
SELECT soldier_id, full_name, rank_name, unit_name, location, status
FROM v_soldier_profile
WHERE unit_name = 'Delta Company'
ORDER BY full_name ASC;