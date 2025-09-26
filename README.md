This tutorial walks through building department‑scoped network file shares in an AD domain using Share & NTFS permissions and group‑based access (RBAC). You’ll create the shares, configure a department security group, validate client access via \\dc-1, and prove access changes by updating group membership. 

<h1>Network File Shares and Permissions</h1>

<h2>Environments and Technologies Used</h2>

- Active Directory Domain Services
- Windows File Sharing
- NTFS Permissions
- Security Groups
- Admin Tools

<h2>Operating Systems Used </h2>

- Windows Server 2022
- Windows 10 (22H2)

<h2>High-Level Deployment and Configuration Steps</h2>

- Create folders for network sharing: read-access, write-access, no-access, and accounting.

- Create new AD Security Group named ACCOUNTANTS

- Set the following permissions on each folder:
  - Grant Domain Users = **Read** on read-access.
  - Grant Domain Users = **Read/Write** on write-access.
  - Grant Domain Admins = **Read/Write** on no-access.
  - Grant ACCOUNTANTS = **Read/Write** on accounting.
- Test permissions for each folder

- Add the user to Security Group ACCOUNTANTS and re‑test access to \\dc-1\accounting to confirm group‑based authorization.


<h2>Deployment and Configuration Steps</h2>

## Step 1: Create AD Security Group 
- We are going to make a new OU for the Accounting department called **ACCOUNTANTS**
- Log into your domain controller machine as a Domain Admin
- Open up the **Active Directory Users and Computers** window
- Create a new Organizational Unit called **_GROUPS**
- Navigate into the **_GROUPS** folder
- Right click in the empty box to the right -> New -> Group (screenshot below)

<img width="1063" height="560" alt="Screenshot 2025-09-25 at 6 56 10 PM" src="https://github.com/user-attachments/assets/4279d1ca-cef6-4b9c-a032-2d764da00e93" />

- Name the group **ACCOUNTANTS**
- Set the **Group Scope** to Global
- Set the **Group Type** to Security
- Click OK

<img width="428" height="364" alt="Screenshot 2025-09-25 at 6 57 05 PM" src="https://github.com/user-attachments/assets/fcbb2667-e412-48be-b500-8b88baf0f443" />

- Navigate back to the C:\ drive
- Open up properties on the **accounting** folder
- Click the Sharing tab -> Click Share
- Type in **ACCOUNTANTS** -> Click Add
- Set **read/write** permissions
- Click Share

<img width="605" height="436" alt="Screenshot 2025-09-25 at 7 09 00 PM" src="https://github.com/user-attachments/assets/27adf425-95ad-492e-a393-331d69704e91" />


## Step 2: Create Folders 

- Log into your Domain Controller (**DC-1**) as a domain admin.
- Open up File Explorer
- Navigate to your C:\ Drive
- Create 4 folders in here called: **read-access**, **write-access**, **no-access**, and **accounting** 

<img width="511" height="95" alt="Screenshot 2025-09-25 at 6 14 10 PM" src="https://github.com/user-attachments/assets/beb39bd2-784c-412a-9ad7-301d627580c6" />

## Step 3: Apply Share/NTFS Permissions to Folders

- Next we are going to assign the proper permissions to each folder we just created.
- Let's assign permissions to **read-access**
- Right-click on the read-access folder -> Click Properties
- Click the Sharing tab -> Click Share

<img width="361" height="477" alt="Screenshot 2025-09-25 at 6 17 56 PM" src="https://github.com/user-attachments/assets/d66cea74-bfad-4860-b2bd-124b2b2e7d8e" />

- Type **Domain Users** in the text box
- Click add
- It should have added **Domain Users** with read access already.
- If not, please change it to read-access.
- Click Share

<img width="576" height="249" alt="Screenshot 2025-09-25 at 6 20 42 PM" src="https://github.com/user-attachments/assets/c9d5c2dc-98c5-4f5b-a5a9-fca82441cd50" />

- If successful you should get this popup. (screenshot below)

<img width="608" height="439" alt="Screenshot 2025-09-25 at 6 21 47 PM" src="https://github.com/user-attachments/assets/dbc4e3b7-8f78-4751-af1c-58879c6fe700" />


- Navigate back to the File Explorer where you have your C:\ drive path
- We're going to assign **read/write** permissions to the **write-access** folder
- Right-click the write-access folder -> Click Properies
- Click the Sharing tab -> Click Share
- Type **Domain Users** in the text box
- Click add
- Click the arrow on Permission level for Domain users
- Set the permission to **read/write**
- Click Share

<img width="575" height="254" alt="Screenshot 2025-09-25 at 6 22 44 PM" src="https://github.com/user-attachments/assets/1d8b8e43-c79b-43d0-a802-fb5f0feb8725" />

- If successful you should get this popup (screenshot below)

<img width="607" height="441" alt="Screenshot 2025-09-25 at 6 25 36 PM" src="https://github.com/user-attachments/assets/25aa4b73-a4a8-4e25-a85b-487f1236bc9e" />

- Let's do the same thing with the **no-access** folder
- For this folder we only want **Domain Admins** to have **read/write** permissions
- At this point I'm assuming you can get to the part where we set permisssions
- Type in domain admins -> Click add -> Set to **read/write** permissions (screenshot below)
- Notice how we only set those permissions for Domain Admins and not Domain Users.
- Consequently, if a domain user tries to open this folder they will be denied access.

<img width="612" height="421" alt="Screenshot 2025-09-25 at 6 27 44 PM" src="https://github.com/user-attachments/assets/80c74e89-67cc-49ef-8f00-2d4faaef1a4e" />

- Lastly, let's set permissions for the **accounting** folder
- Open up properties on the **accounting** folder
- Click the Sharing tab -> Click Share
- Type in **ACCOUNTANTS** -> Click Add
- Set **read/write** permissions
- Click Share

<img width="605" height="436" alt="Screenshot 2025-09-25 at 7 09 00 PM" src="https://github.com/user-attachments/assets/27adf425-95ad-492e-a393-331d69704e91" />

## Step 4: Domain User - Navigating to the Shared Network Resources

- Log into the client machine as bob.min (our domain user)
- Navigate to File Explorer
- In the search bar of the file explorer type **\\dc-1** to access the Shared Folders
- the \\ is telling the File Explorer that you are accessing a **network resource**, which in this case is **dc-1**

<img width="88" height="30" alt="Screenshot 2025-09-25 at 6 31 31 PM" src="https://github.com/user-attachments/assets/c7938292-d33e-4885-a2ab-afdae231039e" />

- You should be in the directory that shows you the **Shared Network Resources**
- You can see the folders we made earlier with assigned permissions (screenshot below)
  
<img width="708" height="117" alt="Screenshot 2025-09-25 at 7 22 20 PM" src="https://github.com/user-attachments/assets/95bb1db5-056e-4a77-96de-6fcd8249bfc9" />

## Step 5: Testing Permissions as Domain User
- bob.min is a member of **Domain Users**
- We are going to test bob.min's permissions inside of each folder

- Testing **read-accesss** folder. This folder allows members of Domain Users to **read only**
  - bob.min attempts to create a new file inside of this folder.
  - Access denied! (screenshot below)
  
<img width="482" height="241" alt="Screenshot 2025-09-25 at 6 44 23 PM" src="https://github.com/user-attachments/assets/e5dda8ec-bdfc-4c4f-b209-99c14379a009" />


- Testing **write-access** folder. This folder allows members of Domain Users to **read/write**
  - bob.min attempts to create a new file called test.txt
  - <img width="703" height="91" alt="Screenshot 2025-09-25 at 6 47 34 PM" src="https://github.com/user-attachments/assets/25f606e8-809d-4d18-8667-5ee6cbddbe83" />
  - Success!
    
  - bob.min attempts to write something inside of the test.txt file and attempts to save
  - <img width="619" height="32" alt="Screenshot 2025-09-25 at 6 48 13 PM" src="https://github.com/user-attachments/assets/9fdab5ae-8633-459d-ab28-ed0357a6f6fd" />
  - Success!


- Testing **no-access** folder. This folder only allows Domain Admins to **read/write**
  - bob.min attempts to open the folder
  - <img width="515" height="191" alt="Screenshot 2025-09-25 at 6 50 31 PM" src="https://github.com/user-attachments/assets/f1abdcba-74c5-4f6d-b7bd-49e4b1dd0c60" />
  - Failure! (Well it's a success in terms of the permissions being set correctly.)
 
- Testing **accounting** folder. This folder only allows the group ACCOUNTANTS to **read/write**
  - bob.min attempts to open the folder
  - <img width="516" height="188" alt="Screenshot 2025-09-25 at 7 23 53 PM" src="https://github.com/user-attachments/assets/79b4bf22-97c3-492d-a71f-6c9bab60db03" />
  - bob.min cannot access this folder since he is not a member of the ACCOUNTANTS group.
  - Let's make Bob a member of the ACCOUNTANTS group

## Step 6: Updating Group Membership for the User

- Log bob.min out of the client machine for now
- Log onto the Domain Controller as a Domain Admin
- Navigate to the **Active Directory Users and Computers** window
- Navigate to the **_GROUPS** folder you made earlier
- Double-click the **ACCOUNTANTS** security group
- Click the Members tab -> Click add
- Type bob.min -> Click Check Names

<img width="448" height="247" alt="Screenshot 2025-09-25 at 7 30 14 PM" src="https://github.com/user-attachments/assets/d8f46166-07ed-4279-b2bb-68bbb8aba052" />

- Click Apply -> Click OK

<img width="396" height="444" alt="Screenshot 2025-09-25 at 7 31 02 PM" src="https://github.com/user-attachments/assets/b71edc07-e349-4be8-83de-11376d38c784" />

## Step 7: Re-testing Folder Access After Adding User to Security Group

- Navigate to the File Explorer
- type \\dc-1 in the file explorer search bar
- You should see all of the shared folders

<img width="764" height="132" alt="Screenshot 2025-09-25 at 7 34 29 PM" src="https://github.com/user-attachments/assets/5870cfe7-799f-4a88-b359-0edc1ee76efc" />

- Double-click the accounting folder
- Success!
- Create a new text file, write something in it, then hit save.
- Success!

<img width="693" height="93" alt="Screenshot 2025-09-25 at 7 35 37 PM" src="https://github.com/user-attachments/assets/f73831af-2917-46c8-b9b3-1bfc4b322289" />

- bob.min was able to successfully use the **read/write** permissions for the accounting folder


## Summary:

- Built domain‑based file sharing on Windows Server with least‑privilege access across four shares (read, write, no‑access, department‑only).

- Modeled role‑based access control (RBAC) using an AD Security/Global group (ACCOUNTANTS) instead of per‑user permissions.

- Applied and validated Share & NTFS permissions to enforce read vs. modify behaviors.

- Demonstrated client access over UNC (\\dc-1) and verified permission outcomes (deny, read‑only, read/write).

- Verified that group membership changes take effect by re-testing folder access after adding a user.

