# SN API KB

What roles needed for CMDB Instance API , ITIL role related query - Support and Troubleshooting
KB2655604
What roles needed for CMDB Instance API , ITIL role related query
426 Views Last updated : Dec 12, 2025 public Copy Permalink
KB Summary by Now Assist
Issue

Which role needed for integration that requires the ServiceNow CMDB instance API.

We have below given official documentation:
https://www.servicenow.com/docs/bundle/zurich-api-reference/page/integrate/inbound-rest/concept/cmdb-instance-api.html

Which says: "

The CMDB Instance API provides endpoints to create, read, update, and delete operations on existing Configuration Management Database (CMDB) tables. You can only insert, update, or delete one record at a time.

This API requires the user have the ITIL role."

Why does the CMDB API require the itil role, where a more suitable role might be appropriate

A more suitable cmdb_role is required as it's already part of ITIL role to access CMDB as below, why can't we have just the below roles for integration user to access only CMDB related tables:
cmdb_read
cmdb_ms_editior
sn_cmdb_user
cmdb_query_builder
cmdb_ms_user
sn_cmdb_editor
Release

After Zurich.
Resolution

We have below given official documentation:
https://www.servicenow.com/docs/bundle/zurich-api-reference/page/integrate/inbound-rest/concept/cmdb-instance-api.html

This API requires the user have the ITIL role.

This is is still valid till Zurich release, In future ITIL role will not be needed.

We have below given story:
STRY60991519 : Add and Replace SYS_SECURITY_ACL_ROLEs within plugin COM.SNC.CMDB (DIRS0000319 - Granular Admin Roles)


"This work consists mainly of:
- Removing ADMIN role from tables and other assets (like Rest Endpoints) . Wherever there is a configuration to ADMIN role, we have to replace it by SN_CMDB_ADMIN role.
- For tables that have ITIL_ADMIN role, we have to add SN_CMDB_ADMIN role. The ITIL_ADMIN role will be removed in a future release.
- For tables that have ITIL role, we have to add SN_CMDB_EDITOR.

 

customers can add the sn_cmdb_admin / sn_cmdb_editor roles to their REST Endpoint ACL for  now. The table/record level ACLs will pass, once they  pass the REST API ACL.

/sys_security_acl.do?sysId=bffaafa05310030026e2099c73dc3443

 

The world works with ServiceNow.
Was this content helpful?
Sign in for more! There's more content available only to authenticated users Sign in for more!
Did this KB article help you?
Did this KB article help you?

    Terms and conditions
    Privacy statement
    GDPR
    Cookie policy
    Cookie preferences

    © 2026 ServiceNow. All rights reserved.

