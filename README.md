# Student-Record-Management-System-with-Users-and-Permissions

## Introduction
This project models how a real secondary school safeguards academic data on a Linux server. You’ll build a small, file-based Student Record Management System with an official records/ repository containing student_list.txt, grades.txt, and attendance.txt, and secure it using Unix users, groups, and permissions.
Operationally, three roles drive the workflow: the teacher maintains the class register and attendance and proposes updates; the exam officer reviews and validates changes—especially grades—and publishes approved updates to the official records; the student has read-only access to published outputs (e.g., an exports/ area) and never writes to the source data. Access follows least-privilege practice with a group-owned directory and inherited group permissions to keep collaboration controlled and auditable.

## Statement of Problem
Student records are scattered across devices and open shared folders without proper access control. Anyone can change files without trace, versions conflict, and private data can leak. When files go missing or get corrupted, there’s no reliable backup, causing disputes and delays.

## Research Objectives
- To work in the terminal: create folders/files and navigate the filesystem using basic commands.
- To manage identities: create users and groups for teacher, exam_officer, and students.
- To control access: set ownership and permissions on records/ so staff can edit and students are read-only.
- To verify settings: quickly check who owns what and what each role can do.

## Project Steps
i.	Download Git Bash

ii.	Launch an EC2 Linux server & SSH into it

iii.	Open terminal & create project root folder with records/ + three files

iv.	Create Linux users & group (real Linux/WSL)

v.	Set ownership & permissions on records

## Project Implementation

### Step One: Download Git Bash

Git Bash is a Windows program that installs Git and a Bash (Unix-style) terminal, giving you a command line with many Linux-like tools so you can run Git and basic shell commands on Windows (it emulates Bash but isn’t full Linux).

- Go to the official download website for git bash. Paste the below in your browser
```bash
https://git-scm.com/downloads
```

- Select Download for Windows
  <img width="974" height="445" alt="image" src="https://github.com/user-attachments/assets/b686bdbc-b066-42ad-9a51-f7e799ecf068" />

 

- Select Git for Windows/x64 Setup
  <img width="975" height="356" alt="image" src="https://github.com/user-attachments/assets/259ea0c0-0ca5-4b91-a8e0-90319b6be00f" />

 

The exe file would download to your computer. Double-click the downloaded git bash exe to install.

- Your Git Bash terminal is ready
  <img width="975" height="308" alt="image" src="https://github.com/user-attachments/assets/a8613690-f209-45b3-ac5f-ded6d69597ba" />

 
### Step 2: Create the EC2 instance (AWS Console)

- Sign in to AWS and Launch it
 <img width="975" height="398" alt="image" src="https://github.com/user-attachments/assets/8e3883c2-f4dd-4b62-9f88-064be02b5ec1" />


- Go to EC2 → Instances → Launch instances.
 <img width="975" height="326" alt="image" src="https://github.com/user-attachments/assets/0fe7ad55-6685-4a83-bf3c-acaabe5eee67" />


- Name: student-records-lab
 <img width="975" height="385" alt="image" src="https://github.com/user-attachments/assets/f28550f8-0286-4325-a3ae-e57989f3897d" />

- AMI (OS): Ubuntu Server 24.04 LTS (recommended for our commands).
 <img width="975" height="401" alt="image" src="https://github.com/user-attachments/assets/54284ca5-4027-4ccb-9d9a-d6c32dd9d452" />


- Instance type: t3.micro (Free Tier eligible).
 <img width="975" height="195" alt="image" src="https://github.com/user-attachments/assets/a23fbc93-86c0-4f02-a911-54322f61aea6" />


- Key pair: Create new key pair → Type: RSA → Format: .pem → Download it (keep safe).
 <img width="975" height="194" alt="image" src="https://github.com/user-attachments/assets/5827eee7-f264-45a8-a9fc-3ab5412cfe11" />


- Click Launch instance 
 <img width="975" height="239" alt="image" src="https://github.com/user-attachments/assets/523d9eaa-e735-488c-b085-52309fd386f4" />


Next, we would need to SSH into our AWS Instance from our git bash

- Open your git-bash and run this command
```bash
cd Downloads
 ```
<img width="933" height="164" alt="image" src="https://github.com/user-attachments/assets/191d09ca-f8c2-451f-8751-05904047ca3b" />


- Then run this with the Public IP of your instance to ssh into it
```bash
ssh -i "C:\Users\<YOU>\Downloads\mykey.pem" ec2-user@<PUBLIC_IP>
```
<img width="975" height="237" alt="image" src="https://github.com/user-attachments/assets/4d72249e-65b8-438b-b4ab-f70d98308541" />



### Step 3: Open terminal & create project root folder with records/ + three files

- Create the project root folder named student-records. Run the below command
```bash
mkdir student-records
```
<img width="975" height="160" alt="image" src="https://github.com/user-attachments/assets/8d91e5a5-3d00-41bb-a627-f1a94b37d5c4" />


- Run this command to move into the just created student-records folder.
```bash
cd student-records
```
<img width="761" height="102" alt="image" src="https://github.com/user-attachments/assets/77c1d55f-beba-401f-bdc3-e91a9096870e" />
 
- Create a folder called records inside the student-records folder. Run;
```bash
mkdir records
```
 <img width="711" height="106" alt="image" src="https://github.com/user-attachments/assets/b17c166a-e0f3-4d93-a63a-b836564cbfee" />

- Run this command to move into the just created records folder.
```bash
cd records
```
<img width="845" height="78" alt="image" src="https://github.com/user-attachments/assets/4bbdd551-a353-42bc-83d7-4f4a8f9c8974" />


- Create the student list file. Makes an empty file named student_list.txt. Run;
```bash
touch student_list.txt
```
- Create the grade file. Makes an empty file named grades.txt. Run;
```bash
touch grades.txt
```
- Create the attendance file. Makes an empty file named attendance.txt. Run;
```bash
touch attendance.txt
``` 
<img width="975" height="155" alt="image" src="https://github.com/user-attachments/assets/42d4712e-fae3-4629-a7e6-ea12697e11c6" />

- To verify that all these files were created, run the command;
```bash
ls
```
 <img width="894" height="159" alt="image" src="https://github.com/user-attachments/assets/1064135b-cba4-430c-a215-2e87d3585311" />


- Add a header to the student list (column names). Run;
```bash
echo "ID,Name,Class" > student_list.txt
```
- Add a header to the grades file. Run;
```bash
echo "ID,Subject,Score" > grades.txt
```
- Add a header to the attendance file. Run;
```bash
echo "ID,Date,Present" > attendance.txt
```
<img width="975" height="244" alt="image" src="https://github.com/user-attachments/assets/ed7ea9d6-346c-4c0a-b42e-0149d9e09bfc" />



- Add student row 1 to row 3. Run these commands one after another.
```bash
echo "1,John Doe,SS2" >> student_list.txt
echo "2,Jane Obi,SS1" >> student_list.txt
echo "3,Sani Musa,SS3" >> student_list.txt
``` 
<img width="975" height="187" alt="image" src="https://github.com/user-attachments/assets/70c59985-5803-4548-8695-06885e331cf3" />

- Add grade row for student ID 1 to student ID 3. Run these commands one after another.
```bash
echo "1,Math,85" >> grades.txt
echo "2,English,92" >> grades.txt
echo "3,Biology,78" >> grades.txt
```
<img width="975" height="185" alt="image" src="https://github.com/user-attachments/assets/9a617180-e159-49bf-b34b-b5e21a0781da" />
 

- Add attendance for student ID 1 to student ID 3. Run these commands one after another.
```bash
echo "1,2025-08-28,Yes" >> attendance.txt
echo "2,2025-08-28,Yes" >> attendance.txt
echo "3,2025-08-28,No" >> attendance.txt
``` 
<img width="975" height="157" alt="image" src="https://github.com/user-attachments/assets/0e234165-9081-439d-b247-16abee63f243" />

- Preview the student list. Run;
```bash
head -5 student_list.txt
``` 
<img width="975" height="173" alt="image" src="https://github.com/user-attachments/assets/d231d169-8cc3-416a-85b4-79043411a0ef" />




- head -5 grades.txt. Run;
```bash
head -5 grades.txt
```
<img width="975" height="195" alt="image" src="https://github.com/user-attachments/assets/87025243-cae7-41ad-9487-c4d329bf2152" />


- Preview the attendance file. Run;
```bash
head -5 attendance.txt
```
<img width="975" height="158" alt="image" src="https://github.com/user-attachments/assets/498d4e6b-9f76-418e-94e2-924da296648b" />


### Step 4: Create Linux users & group (real Linux/WSL)
In this step, we’ll create 
i.	Users: teacher, exam_officer, student

ii.	Groups:
      - records_staff → for teacher + exam_officer (write/review rights to official records/)
      - students → for student accounts (read-only access to exports/ later)
      

- Create the two groups, the records_staff and students group. Run these commands one after another
```bash
sudo groupadd records_staff
sudo groupadd students
``` 
<img width="975" height="152" alt="image" src="https://github.com/user-attachments/assets/e732d9bd-3e1f-40f8-acc5-94af61e3fbda" />



- Creates three user accounts, teacher, exam_officer, student. Run these commands;
```bash
sudo adduser --disabled-password --gecos "" teacher
```
<img width="975" height="223" alt="image" src="https://github.com/user-attachments/assets/30bd9a0f-96f7-46bf-8fff-b21f13ef47cc" />


```bash
sudo adduser --disabled-password --gecos "" exam_officer
```
<img width="975" height="213" alt="image" src="https://github.com/user-attachments/assets/5c3fee62-9bb1-4e5d-8d2b-e33476fb2580" />


```bash
sudo adduser --disabled-password --gecos "" student
```
<img width="975" height="286" alt="image" src="https://github.com/user-attachments/assets/bdff0a09-521f-4448-881e-34ae6f0d4d01" />


- Now, let us add users to the right groups. Run these commands one after the other
```bash
sudo adduser teacher records_staff
sudo adduser exam_officer records_staff
sudo adduser student students
```
<img width="975" height="235" alt="image" src="https://github.com/user-attachments/assets/49e8c648-cf02-4fb9-b56a-0d48e557a44a" />


- Let us confirm our users and groups are created correctly. Run;
```bash
while IFS=: read -r user _; do printf "%s: " "$user"; id -nG "$user"; done < /etc/passwd
``` 
<img width="897" height="112" alt="image" src="https://github.com/user-attachments/assets/70f01ac4-133a-429f-92d8-05af17992dcb" />


### Step 5: Set ownership & permissions on records
In this step, we make staff (records_staff) able to read & edit records/, and students (students) read-only.

- Allow staff (records_staff) to read & edit; set exam_officer as owner. Run the below command; the command recursively changes the owner of the records folder and everything inside it to the user exam_officer and the group records_staff.
```bash
sudo chown -R exam_officer:records_staff .
```
<img width="975" height="116" alt="image" src="https://github.com/user-attachments/assets/7b0afab9-9b1c-468f-907a-0cd7190f2793" />


- Now, make records/ a private workspace where only the owner and staff group can fully work, and group ownership sticks for future content. Run;
```bash
sudo chmod 2770 .
```
<img width="975" height="106" alt="image" src="https://github.com/user-attachments/assets/de3b8dfa-f1bf-40bb-96b8-4c5c68b413cf" />
 

- This command makes sure the staff group can read, edit, and navigate everything inside records/, without accidentally making normal text files executable.
```bash
sudo chmod -R g+rwX .
```
<img width="975" height="111" alt="image" src="https://github.com/user-attachments/assets/a4a7553e-7956-40c0-b85a-394e085ce160" />


- Update your linux terminal. Run;
```bash
sudo apt-get update
sudo apt-get install -y acl
```



- Allow students (students) to read-only. The below command makes all student accounts read-only visitors to the records/ directory and its contents.
```bash
sudo setfacl -R -m g:students:rx records
sudo setfacl -d -m g:students:rx records
```
 <img width="975" height="177" alt="image" src="https://github.com/user-attachments/assets/8e467339-024b-4909-8ddb-8a05cc533cfb" />


- Verify
```bash
ls -ld records
getfacl -p records | sed -n '1,20p'
```
<img width="975" height="357" alt="image" src="https://github.com/user-attachments/assets/15b2beaa-2053-4559-88b2-cbeca30359bf" />


The above verification shows,

- Staff (exam_officer + records_staff) can read and write.
- Students can read-only (no writing).
- Others are completely locked out.
- The setgid + default ACLs guarantee that these permissions apply consistently to any new files created inside.

## Conclusion
The Student Record Management System was successfully implemented using Linux users, groups, permissions, and ACLs to model a real school workflow. Teachers and the exam officer collaborate securely under the records_staff group, with the exam officer as owner to validate and publish updates. Students are restricted to read-only access, ensuring they can view but never alter official data. By combining group ownership, the setgid bit, and default ACLs, the system enforces least-privilege access, prevents unauthorized changes, and guarantees that future files inherit the correct rules. This provides a simple yet effective way to protect academic records, maintain integrity, and support reliable collaboration.
