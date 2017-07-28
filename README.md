# ZenAgent Suite
Zendesk User Management Made (kind of) Simpler!

## What is it?
A terminal program that bundles a number of agent account management processes.

## Scenario

+ Our central system generates Zendesk user accounts when any new user signs up at on our site, including new support agents.
+ Because of this, we are unable to change/remove default group assignments,add new groups, or set passwords en masse.
+ With large classes, the manual process can be time consuming, cumbersome, and at worst, inaccurate.
+ Because User Management requires Full Admin Access in Zendesk, a limited number of employees can execute this process.

Battle-tested at [Instacart](https://www.instacart.com/opensource)

## Programs

### Initial Access Bulk Creation

#### What does it do? 
+ Set agents to any custom role
+ Set user field values
+ Create/Destroy Group Memberships (including the default)
+ Set a password for new agents

#### How Does it Work?

The user passes in a csv with the email address and agent field options.

### Usage

Make Sure to Setup your system to use the [Zendesk Ruby Client](https://github.com/zendesk/zendesk_api_client_rb).

Visit the [User API Reference in Zendesk's Developer Pages](https://developer.zendesk.com/rest_api/docs/core/users) for usage tips and user field syntax.

1. I store my ZD username and API token locally in .bash_profile as `export ZD_USERNAME="myactualemail@punked.com"` and `export ZD_TOKEN="REALLYLONGSTRINGOFCHARACTERS"`, respectively. 

2. Prepare a CSV with the user emails in the first column, and any user fields required. Make sure to add a title row, as the first command will remove this row. Maybe prepare a version with only 1 or 2 rows at first to make sure the program does what you want. 
_**It's super important that your CSV file is formatted accurately. It will be used to extract a `user_emails` array that is then used to run a query for `user_ids`. The resulting `user_ids` are pushed as a new "column" into the csv, but also kept as a separate array to be used in several of the other functions. Check out the [Sample CSV](zendesk_agent_management_suite/sample.csv).**_

3. Define your config variables (`filename`, `custom_role_id`, `password`, `new_default_group_id`, etc. at the the top of the file.
_**Make sure you have to correct path for the csv file if it's not in the same directory as your program file.**_
    

4. I recommend not publishing the password in any persisted records and giving the password to any trainers or leads privately to avoid any potential security breaches.

5. Adjust the indices and custom field names in the `add_user_fields` method.

6. Check your work to avoid annoying manual fixes!

7. Call the program file `ruby zd_bulk_agent_creation.rb` from your command line. The program will take a few minutes to complete. While the confirmation messages for each step add extra time, I've found they will help you debug much faster if you do encounter an error. 

#### Optional Settings
+ You may add or remove any number of user fields or group memberships. Just adjust your variables and indices!
+ You may comment out any user-profile impacting methods that are not needed. For example, if you don't have any user fields to edit, just comment out `add_user_fields(user_fields, client)`. However, be aware that because there is only one method that runs a query (`get_user_ids`), all methods that push changes to the actual user require `get_user_ids`
    * `get_user_ids` requires `extract_user_emails`


#### Limitations
+ Only works for existing users (planning to fix this in V2 by conditioning creation if user is not found)
+ If run and an error occurs due to a missing user, previous changes will not be reverted (planning to fix this in V2)

#### V2 Plans
+ Convert to a Zendesk app that can be used by assigned roles
+ Allow users to select values for each option, such as which role, which fields, desired password, etc.
+ Set unique passwords and email the users
+ Change primary email if user entered it incorrectly initially
+ Correct Name Capitalization
+ Store username and token in the app itself for use by anyone

### Other Programs In the Suite (In Development)
+ Changes Suspended Users to End Users when their tickets have closed
+ Changes role and group assignments for a single user (for promotions and the like)




